## Reduce deployment cost by using EIP1167 minimal clones in `TimeswapV2PoolDeployer` and `TimeswapV2OptionDeployer`

Currently, both `TimeswapV2OptionDeployer` and `TimeswapV2PoolDeployer` create `TimeswapV2Option`s and `TimeswapV2Pool`s by deploying a new contract each time a new pool/option is created. However, this pattern leads to the need of paying for storage costs each time a pool is deployed. 

See TimeswapV2PoolDeployer.sol:

```solidity
Line 28:
          poolPair = address(new TimeswapV2Pool{salt: keccak256(abi.encode(optionPair))}());
```

See TimeswapV2OptionDeployer.sol:

```solidity
Line 35:
          optionPair = address(new TimeswapV2Option{salt: keccak256(abi.encode(token0, token1))}());
```

The same behavior can be kept while producing EIP1167 minimal clones for an implementation contract, drastically reducing deployment costs.

### Tests gas output

There's a significant test in `/packages/v2-pool/test/libraries/PoolFactory.sol` used as a measurement to compare gas savings via tests.

#### Current implementation

```
[PASS] testGetWithCheckNotZero() (gas: 6368937)
```

#### EIP1167 Pool Clones

```
[PASS] testGetWithCheckNotZero() (gas: 2946936)
```

### EIP1167 Option Clones

```
[PASS] testGetWithCheckNotZero() (gas: 3849560)
```

#### EIP1167 Pool and Option Clones

```
[PASS] testGetWithCheckNotZero() (gas: 449275)
```

This method achieves ~7% of the original gas costs.

### Diff

In order to change the behaviour to EIP1167 clones, it's needed to remove some constructors down in the `TimeswapV2PoolFactory` inheritance chain to initializers, so they can be called upon cloning.

Pertinent checks are added and only the diff for `TimeswapV2PoolDeployer` since `TimeswapV2OptionDeployer` became obvious.

```diff
diff --git a/packages/v2-pool/src/NoDelegateCall.sol b/packages/v2-pool/src/NoDelegateCall.sol
index 1045147..f6c3fb7 100644
--- a/packages/v2-pool/src/NoDelegateCall.sol
+++ b/packages/v2-pool/src/NoDelegateCall.sol
@@ -12,11 +12,11 @@ contract NoDelegateCall {
     /* ===== MODEL ===== */
 
     /// @dev The original address of this contract.
-    address private immutable original;
+    address private original;
 
     /* ===== INIT ===== */
 
-    constructor() {
+    function initialize() public virtual {
         // Immutables are computed in the init code of the contract, and then inlined into the deployed bytecode.
         // In other words, this variable won't change when it's checked at runtime.
         original = address(this);
diff --git a/packages/v2-pool/src/TimeswapV2Pool.sol b/packages/v2-pool/src/TimeswapV2Pool.sol
index dfedce9..84171e5 100644
--- a/packages/v2-pool/src/TimeswapV2Pool.sol
+++ b/packages/v2-pool/src/TimeswapV2Pool.sol
@@ -41,18 +41,19 @@ contract TimeswapV2Pool is ITimeswapV2Pool, NoDelegateCall {
     /* ===== MODEL ===== */
 
     /// @inheritdoc ITimeswapV2Pool
-    address public immutable override poolFactory;
+    address public override poolFactory;
     /// @inheritdoc ITimeswapV2Pool
-    address public immutable override optionPair;
+    address public override optionPair;
     /// @inheritdoc ITimeswapV2Pool
-    uint256 public immutable override transactionFee;
+    uint256 public override transactionFee;
     /// @inheritdoc ITimeswapV2Pool
-    uint256 public immutable override protocolFee;
+    uint256 public override protocolFee;
 
     mapping(uint256 => mapping(uint256 => uint96)) private reentrancyGuards;
     mapping(uint256 => mapping(uint256 => Pool)) private pools;
 
     StrikeAndMaturity[] private listOfPools;
+    bool _initialized;
 
     function addPoolEnumerationIfNecessary(uint256 strike, uint256 maturity) private {
         if (reentrancyGuards[strike][maturity] == ReentrancyGuard.NOT_INTERACTED) {
@@ -74,8 +75,11 @@ contract TimeswapV2Pool is ITimeswapV2Pool, NoDelegateCall {
 
     /* ===== INIT ===== */
 
-    constructor() NoDelegateCall() {
+    function initialize() public override {
+        require(!_initialized);
+        super.initialize();
         (poolFactory, optionPair, transactionFee, protocolFee) = ITimeswapV2PoolDeployer(msg.sender).parameter();
+        _initialized = true;
     }
 
     // Can be overidden for testing purposes.
diff --git a/packages/v2-pool/src/TimeswapV2PoolDeployer.sol b/packages/v2-pool/src/TimeswapV2PoolDeployer.sol
index 395b3b4..d781b55 100644
--- a/packages/v2-pool/src/TimeswapV2PoolDeployer.sol
+++ b/packages/v2-pool/src/TimeswapV2PoolDeployer.sol
@@ -5,6 +5,8 @@ import {TimeswapV2Pool} from "./TimeswapV2Pool.sol";
 
 import {ITimeswapV2PoolDeployer} from "./interfaces/ITimeswapV2PoolDeployer.sol";
 
+import {Clones} from "@openzeppelin/contracts/proxy/Clones.sol";
+
 /// @title Capable of deploying Timeswap V2 Pool
 /// @author Timeswap Labs
 contract TimeswapV2PoolDeployer is ITimeswapV2PoolDeployer {
@@ -20,12 +22,20 @@ contract TimeswapV2PoolDeployer is ITimeswapV2PoolDeployer {
     /// @inheritdoc ITimeswapV2PoolDeployer
     Parameter public override parameter;
 
+    address private _implementation;
+
+    constructor() {
+        TimeswapV2Pool implementation = new TimeswapV2Pool();
+        _implementation = address(implementation);
+    }
+
     /* ===== UPDATE ===== */
 
     function deploy(address poolFactory, address optionPair, uint256 transactionFee, uint256 protocolFee) internal returns (address poolPair) {
         parameter = Parameter({poolFactory: poolFactory, optionPair: optionPair, transactionFee: transactionFee, protocolFee: protocolFee});
 
-        poolPair = address(new TimeswapV2Pool{salt: keccak256(abi.encode(optionPair))}());
+        poolPair = Clones.cloneDeterministic(_implementation, keccak256(abi.encode(optionPair)));
+        TimeswapV2Pool(poolPair).initialize();
 
         delete parameter;
     }
diff --git a/packages/v2-pool/src/TimeswapV2PoolFactory.sol b/packages/v2-pool/src/TimeswapV2PoolFactory.sol
index 914d719..cf2884a 100644
--- a/packages/v2-pool/src/TimeswapV2PoolFactory.sol
+++ b/packages/v2-pool/src/TimeswapV2PoolFactory.sol
@@ -34,7 +34,7 @@ contract TimeswapV2PoolFactory is ITimeswapV2PoolFactory, TimeswapV2PoolDeployer
 
     /* ===== INIT ===== */
 
-    constructor(address chosenOwner, uint256 chosenTransactionFee, uint256 chosenProtocolFee) OwnableTwoSteps(chosenOwner) {
+    constructor(address chosenOwner, uint256 chosenTransactionFee, uint256 chosenProtocolFee) OwnableTwoSteps(chosenOwner) TimeswapV2PoolDeployer() {
         if (chosenTransactionFee > type(uint16).max) revert IncorrectFeeInitialization(chosenTransactionFee);
         if (chosenProtocolFee > type(uint16).max) revert IncorrectFeeInitialization(chosenProtocolFee);
 
```
