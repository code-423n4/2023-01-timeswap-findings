### 1st BUG
Don't Initialize Variables with Default Value

#### Impact
Issue Information: 
Uninitialized variables are assigned with the types default value.

Explicitly initializing a variable with it's default value costs unnecessary gas.

Example
🤦 Bad:

uint256 x = 0;
bool y = false;
🚀 Good:

uint256 x;
bool y;

#### Findings:
```
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::231 => for (uint256 i = 0; i < rpcs.length; i++) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::63 => for (uint256 i = 0; i < reads.length; i++) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::169 => for (uint256 i = 0; i < max; i++) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::177 => for (uint256 i = 0; i < b.length; i++) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::299 => for (uint256 i = 0; i < max; i++) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::308 => for (uint256 i = 0; i < b.length; i++) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::63 => for (uint256 i = 0; i < reads.length; i++) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::169 => for (uint256 i = 0; i < max; i++) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::177 => for (uint256 i = 0; i < b.length; i++) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::299 => for (uint256 i = 0; i < max; i++) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::308 => for (uint256 i = 0; i < b.length; i++) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::122 => for (uint256 i = 0; i < strb.length; i++) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::63 => for (uint256 i = 0; i < reads.length; i++) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::169 => for (uint256 i = 0; i < max; i++) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::177 => for (uint256 i = 0; i < b.length; i++) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::299 => for (uint256 i = 0; i < max; i++) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::308 => for (uint256 i = 0; i < b.length; i++) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::121 => for (uint256 i = 0; i < length; ++i) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::131 => for (uint256 i = 0; i < length; ++i) {
```
#### Tools used
Manual VS code audit

### 2nd BUG
Cache Array Length Outside of Loop

#### Impact
Issue Information: 
Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.

Example
🤦 Bad:

for (uint256 i = 0; i < array.length; i++) {
    // invariant: array's length is not changed
}
🚀 Good:

uint256 len = array.length
for (uint256 i = 0; i < len; i++) {
    // invariant: array's length is not changed
}

#### Findings:
```
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::231 => for (uint256 i = 0; i < rpcs.length; i++) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::285 => Tx1559[] memory txs = new Tx1559[](rawTxs.length);
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::286 => for (uint256 i; i < rawTxs.length; i++) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::348 => Receipt[] memory receipts = new Receipt[](rawReceipts.length);
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::349 => for (uint256 i; i < rawReceipts.length; i++) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::374 => ReceiptLog[] memory logs = new ReceiptLog[](rawLogs.length);
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::375 => for (uint256 i; i < rawLogs.length; i++) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::451 => require(b.length <= 32, "StdCheats _bytesToUint(bytes): Bytes length exceeds 32.");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::452 => return abi.decode(abi.encodePacked(new bytes(32 - b.length), b), (uint256));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::51 => if (reads.length == 1) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::62 => } else if (reads.length > 1) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::63 => for (uint256 i = 0; i < reads.length; i++) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::168 => uint256 max = b.length > 32 ? 32 : b.length;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::176 => bytes memory result = new bytes(b.length * 32);
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::177 => for (uint256 i = 0; i < b.length; i++) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::298 => uint256 max = b.length > 32 ? 32 : b.length;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::307 => bytes memory result = new bytes(b.length * 32);
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::308 => for (uint256 i = 0; i < b.length; i++) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdUtils.sol::70 => require(b.length <= 32, "StdUtils bytesToUint(bytes): Bytes length exceeds 32.");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdUtils.sol::71 => return abi.decode(abi.encodePacked(new bytes(32 - b.length), b), (uint256));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::8 => uint256 payloadLength = payload.length;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::13 => uint256 payloadLength = payload.length;
2023-01-timeswap/packages/v2-library/src/BytesLib.sol::12 => function slice(bytes memory _bytes, uint256 _start, uint256 _length) internal pure returns (bytes memory) {
2023-01-timeswap/packages/v2-library/src/BytesLib.sol::13 => require(_length + 31 >= _length, "slice_overflow");
2023-01-timeswap/packages/v2-library/src/BytesLib.sol::14 => require(_bytes.length >= _start + _length, "slice_outOfBounds");
2023-01-timeswap/packages/v2-library/src/BytesLib.sol::19 => switch iszero(_length)
2023-01-timeswap/packages/v2-library/src/BytesLib.sol::33 => let lengthmod := and(_length, 31)
2023-01-timeswap/packages/v2-library/src/BytesLib.sol::39 => let mc := add(add(tempBytes, lengthmod), mul(0x20, iszero(lengthmod)))
2023-01-timeswap/packages/v2-library/src/BytesLib.sol::40 => let end := add(mc, _length)
2023-01-timeswap/packages/v2-library/src/BytesLib.sol::45 => let cc := add(add(add(_bytes, lengthmod), mul(0x20, iszero(lengthmod))), _start)
2023-01-timeswap/packages/v2-library/src/BytesLib.sol::53 => mstore(tempBytes, _length)
2023-01-timeswap/packages/v2-library/src/CatchError.sol::13 => uint256 length = reason.length;
2023-01-timeswap/packages/v2-library/src/CatchError.sol::15 => if ((length - 4) % 32 == 0 && bytes4(reason) == selector) return BytesLib.slice(reason, 4, length - 4);
2023-01-timeswap/packages/v2-library/src/CatchError.sol::18 => revert(reason, length)
2023-01-timeswap/packages/v2-option/src/TimeswapV2Option.sol::81 => return listOfOptions.length;
2023-01-timeswap/packages/v2-option/src/TimeswapV2Option.sol::178 => if (param.data.length != 0)
2023-01-timeswap/packages/v2-option/src/TimeswapV2Option.sol::265 => if (param.data.length != 0)
2023-01-timeswap/packages/v2-option/src/TimeswapV2OptionFactory.sol::37 => return getByIndex.length;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::63 => require(bytes(chainAlias).length != 0, "StdChains getChain(string): Chain alias cannot be the empty string.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::86 => require(bytes(chainAlias).length != 0, "StdChains setChain(string,Chain): Chain alias cannot be the empty string.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::94 => bytes(foundAlias).length == 0 || keccak256(bytes(foundAlias)) == keccak256(bytes(chainAlias)),
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::108 => if (bytes(chain.rpcUrl).length == 0) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::115 => if (keccak256(notFoundError) != keccak256(err) || bytes(chain.rpcUrl).length == 0) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdCheats.sol::239 => Tx1559[] memory txs = new Tx1559[](rawTxs.length);
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdCheats.sol::240 => for (uint256 i; i < rawTxs.length; i++) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdCheats.sol::302 => Receipt[] memory receipts = new Receipt[](rawReceipts.length);
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdCheats.sol::303 => for (uint256 i; i < rawReceipts.length; i++) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdCheats.sol::328 => ReceiptLog[] memory logs = new ReceiptLog[](rawLogs.length);
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdCheats.sol::329 => for (uint256 i; i < rawLogs.length; i++) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdCheats.sol::405 => require(b.length <= 32, "StdCheats _bytesToUint(bytes): Bytes length exceeds 32.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdCheats.sol::406 => return abi.decode(abi.encodePacked(new bytes(32 - b.length), b), (uint256));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::51 => if (reads.length == 1) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::62 => } else if (reads.length > 1) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::63 => for (uint256 i = 0; i < reads.length; i++) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::168 => uint256 max = b.length > 32 ? 32 : b.length;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::176 => bytes memory result = new bytes(b.length * 32);
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::177 => for (uint256 i = 0; i < b.length; i++) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::298 => uint256 max = b.length > 32 ? 32 : b.length;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::307 => bytes memory result = new bytes(b.length * 32);
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::308 => for (uint256 i = 0; i < b.length; i++) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdUtils.sol::95 => require(b.length <= 32, "StdUtils bytesToUint(bytes): Bytes length exceeds 32.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdUtils.sol::96 => return abi.decode(abi.encodePacked(new bytes(32 - b.length), b), (uint256));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/Vm.sol::27 => uint256 length;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::8 => uint256 payloadLength = payload.length;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::13 => uint256 payloadLength = payload.length;
2023-01-timeswap/packages/v2-pool/src/TimeswapV2Pool.sol::95 => return listOfPools.length;
2023-01-timeswap/packages/v2-pool/src/TimeswapV2PoolFactory.sol::53 => return getByIndex.length;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::72 => require(bytes(chainAlias).length != 0, "StdChains getChain(string): Chain alias cannot be the empty string.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::95 => require(bytes(chainAlias).length != 0, "StdChains setChain(string,ChainData): Chain alias cannot be the empty string.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::103 => bytes(foundAlias).length == 0 || keccak256(bytes(foundAlias)) == keccak256(bytes(chainAlias)),
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::121 => bytes memory copy = new bytes(strb.length);
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::122 => for (uint256 i = 0; i < strb.length; i++) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::136 => if (bytes(chain.rpcUrl).length == 0) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::143 => if (keccak256(notFoundError) != keccak256(err) || bytes(chain.rpcUrl).length == 0) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdCheats.sol::239 => Tx1559[] memory txs = new Tx1559[](rawTxs.length);
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdCheats.sol::240 => for (uint256 i; i < rawTxs.length; i++) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdCheats.sol::302 => Receipt[] memory receipts = new Receipt[](rawReceipts.length);
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdCheats.sol::303 => for (uint256 i; i < rawReceipts.length; i++) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdCheats.sol::328 => ReceiptLog[] memory logs = new ReceiptLog[](rawLogs.length);
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdCheats.sol::329 => for (uint256 i; i < rawLogs.length; i++) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdCheats.sol::405 => require(b.length <= 32, "StdCheats _bytesToUint(bytes): Bytes length exceeds 32.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdCheats.sol::406 => return abi.decode(abi.encodePacked(new bytes(32 - b.length), b), (uint256));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::51 => if (reads.length == 1) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::62 => } else if (reads.length > 1) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::63 => for (uint256 i = 0; i < reads.length; i++) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::168 => uint256 max = b.length > 32 ? 32 : b.length;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::176 => bytes memory result = new bytes(b.length * 32);
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::177 => for (uint256 i = 0; i < b.length; i++) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::298 => uint256 max = b.length > 32 ? 32 : b.length;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::307 => bytes memory result = new bytes(b.length * 32);
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::308 => for (uint256 i = 0; i < b.length; i++) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::79 => require(b.length <= 32, "StdUtils bytesToUint(bytes): Bytes length exceeds 32.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::80 => return abi.decode(abi.encodePacked(new bytes(32 - b.length), b), (uint256));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::119 => uint256 length = addresses.length;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::120 => IMulticall3.Call[] memory calls = new IMulticall3.Call[](length);
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::121 => for (uint256 i = 0; i < length; ++i) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::130 => balances = new uint256[](length);
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::131 => for (uint256 i = 0; i < length; ++i) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/Vm.sol::27 => uint256 length;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::8 => uint256 payloadLength = payload.length;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::13 => uint256 payloadLength = payload.length;
2023-01-timeswap/packages/v2-token/src/TimeswapV2LiquidityToken.sol::162 => if (param.data.length != 0)
2023-01-timeswap/packages/v2-token/src/TimeswapV2LiquidityToken.sol::201 => if (param.data.length != 0)
2023-01-timeswap/packages/v2-token/src/TimeswapV2Token.sol::215 => if (param.data.length != 0)
2023-01-timeswap/packages/v2-token/src/base/ERC1155Enumerable.sol::37 => return _allTokens.length;
2023-01-timeswap/packages/v2-token/src/base/ERC1155Enumerable.sol::118 => uint256 length = _currentIndex[to];
2023-01-timeswap/packages/v2-token/src/base/ERC1155Enumerable.sol::119 => _ownedTokens[to][length] = tokenId;
2023-01-timeswap/packages/v2-token/src/base/ERC1155Enumerable.sol::120 => _ownedTokensIndex[tokenId] = length;
2023-01-timeswap/packages/v2-token/src/base/ERC1155Enumerable.sol::126 => _allTokensIndex[tokenId] = _allTokens.length;
2023-01-timeswap/packages/v2-token/src/base/ERC1155Enumerable.sol::155 => uint256 lastTokenIndex = _allTokens.length - 1;
```
#### Tools used
Manual VS code audit

### 3rd BUG
Use != 0 instead of > 0 for Unsigned Integer Comparison

#### Impact
Issue Information: 
When dealing with unsigned integer types, comparisons with != 0 are cheaper than with > 0.

Example
🤦 Bad:

// `a` being of type unsigned integer
require(a > 0, "!a > 0");
🚀 Good:

// `a` being of type unsigned integer
require(a != 0, "!a > 0");

#### Findings:
```
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdMath.sol::13 => return uint256(a > 0 ? a : -a);
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdMath.sol::13 => return uint256(a > 0 ? a : -a);
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdMath.sol::13 => return uint256(a > 0 ? a : -a);
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::116 => require(tokenCodeSize > 0, "StdUtils getTokenBalances(address,address[]): Token address is not a contract.");
```
#### Tools used
Manual VS code audit

### 4th BUG
Use immutable for OpenZeppelin AccessControl's Roles Declarations

#### Impact
Issue Information: 
⚡️ Only valid for solidity versions <0.6.12 ⚡️

Access roles marked as constant results in computing the keccak256 operation each time the variable is used because assigned operations for constant variables are re-evaluated every time.

Changing the variables to immutable results in computing the hash only once on deployment, leading to gas savings.

Example
🤦 Bad:

bytes32 public constant GOVERNOR_ROLE = keccak256("GOVERNOR_ROLE");
🚀 Good:

bytes32 public immutable GOVERNOR_ROLE = keccak256("GOVERNOR_ROLE");

#### Findings:
```
2023-01-timeswap/packages/v2-library/lib/forge-std/src/Common.sol::7 => address internal constant VM_ADDRESS = address(uint160(uint256(keccak256("hevm cheat code"))));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdAssertions.sol::53 => if (keccak256(abi.encode(a)) != keccak256(abi.encode(b))) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdAssertions.sol::62 => if (keccak256(abi.encode(a)) != keccak256(abi.encode(b))) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdAssertions.sol::71 => if (keccak256(abi.encode(a)) != keccak256(abi.encode(b))) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdAssertions.sol::80 => if (keccak256(abi.encode(a)) != keccak256(abi.encode(b))) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdAssertions.sol::87 => if (keccak256(abi.encode(a)) != keccak256(abi.encode(b))) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdAssertions.sol::94 => if (keccak256(abi.encode(a)) != keccak256(abi.encode(b))) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::10 => VmSafe private constant vm = VmSafe(address(uint160(uint256(keccak256("hevm cheat code")))));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::435 => privateKey = uint256(keccak256(abi.encodePacked(name)));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::461 => Vm private constant vm = Vm(address(uint160(uint256(keccak256("hevm cheat code")))));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdJson.sol::30 => VmSafe private constant vm = Vm(address(uint160(uint256(keccak256("hevm cheat code")))));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::20 => Vm private constant vm = Vm(address(uint160(uint256(keccak256("hevm cheat code")))));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::23 => return bytes4(keccak256(bytes(sigStr)));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::39 => if (self.finds[who][fsig][keccak256(abi.encodePacked(ins, field_depth))]) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::40 => return self.slots[who][fsig][keccak256(abi.encodePacked(ins, field_depth))];
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::59 => emit SlotFound(who, fsig, keccak256(abi.encodePacked(ins, field_depth)), uint256(reads[0]));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::60 => self.slots[who][fsig][keccak256(abi.encodePacked(ins, field_depth))] = uint256(reads[0]);
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::61 => self.finds[who][fsig][keccak256(abi.encodePacked(ins, field_depth))] = true;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::79 => emit SlotFound(who, fsig, keccak256(abi.encodePacked(ins, field_depth)), uint256(reads[i]));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::80 => self.slots[who][fsig][keccak256(abi.encodePacked(ins, field_depth))] = uint256(reads[i]);
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::81 => self.finds[who][fsig][keccak256(abi.encodePacked(ins, field_depth))] = true;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::91 => require(self.finds[who][fsig][keccak256(abi.encodePacked(ins, field_depth))], "stdStorage find(StdStorage): Slot(s) not found.");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::98 => return self.slots[who][fsig][keccak256(abi.encodePacked(ins, field_depth))];
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::190 => Vm private constant vm = Vm(address(uint160(uint256(keccak256("hevm cheat code")))));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::252 => if (!self.finds[who][fsig][keccak256(abi.encodePacked(ins, field_depth))]) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::255 => bytes32 slot = bytes32(self.slots[who][fsig][keccak256(abi.encodePacked(ins, field_depth))]);
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdUtils.sol::48 => if (nonce == 0x00) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd6), bytes1(0x94), deployer, bytes1(0x80))));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdUtils.sol::49 => if (nonce <= 0x7f) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd6), bytes1(0x94), deployer, uint8(nonce))));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdUtils.sol::52 => if (nonce <= 2 ** 8 - 1) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd7), bytes1(0x94), deployer, bytes1(0x81), uint8(nonce))));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdUtils.sol::53 => if (nonce <= 2 ** 16 - 1) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd8), bytes1(0x94), deployer, bytes1(0x82), uint16(nonce))));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdUtils.sol::54 => if (nonce <= 2 ** 24 - 1) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd9), bytes1(0x94), deployer, bytes1(0x83), uint24(nonce))));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdUtils.sol::62 => return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xda), bytes1(0x94), deployer, bytes1(0x84), uint32(nonce))));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdUtils.sol::66 => return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xff), deployer, salt, initcodeHash)));
2023-01-timeswap/packages/v2-option/src/TimeswapV2OptionDeployer.sol::35 => optionPair = address(new TimeswapV2Option{salt: keccak256(abi.encode(token0, token1))}());
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/Base.sol::9 => address internal constant VM_ADDRESS = address(uint160(uint256(keccak256("hevm cheat code"))));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/Base.sol::13 => address internal constant DEFAULT_SENDER = address(uint160(uint256(keccak256("foundry default caller"))));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdAssertions.sol::53 => if (keccak256(abi.encode(a)) != keccak256(abi.encode(b))) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdAssertions.sol::62 => if (keccak256(abi.encode(a)) != keccak256(abi.encode(b))) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdAssertions.sol::71 => if (keccak256(abi.encode(a)) != keccak256(abi.encode(b))) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdAssertions.sol::80 => if (keccak256(abi.encode(a)) != keccak256(abi.encode(b))) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdAssertions.sol::87 => if (keccak256(abi.encode(a)) != keccak256(abi.encode(b))) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdAssertions.sol::94 => if (keccak256(abi.encode(a)) != keccak256(abi.encode(b))) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::38 => VmSafe private constant vm = VmSafe(address(uint160(uint256(keccak256("hevm cheat code")))));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::94 => bytes(foundAlias).length == 0 || keccak256(bytes(foundAlias)) == keccak256(bytes(chainAlias)),
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::115 => if (keccak256(notFoundError) != keccak256(err) || bytes(chain.rpcUrl).length == 0) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdCheats.sol::10 => Vm private constant vm = Vm(address(uint160(uint256(keccak256("hevm cheat code")))));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdCheats.sol::389 => privateKey = uint256(keccak256(abi.encodePacked(name)));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdCheats.sol::454 => Vm private constant vm = Vm(address(uint160(uint256(keccak256("hevm cheat code")))));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdJson.sol::30 => VmSafe private constant vm = VmSafe(address(uint160(uint256(keccak256("hevm cheat code")))));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::20 => Vm private constant vm = Vm(address(uint160(uint256(keccak256("hevm cheat code")))));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::23 => return bytes4(keccak256(bytes(sigStr)));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::39 => if (self.finds[who][fsig][keccak256(abi.encodePacked(ins, field_depth))]) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::40 => return self.slots[who][fsig][keccak256(abi.encodePacked(ins, field_depth))];
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::59 => emit SlotFound(who, fsig, keccak256(abi.encodePacked(ins, field_depth)), uint256(reads[0]));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::60 => self.slots[who][fsig][keccak256(abi.encodePacked(ins, field_depth))] = uint256(reads[0]);
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::61 => self.finds[who][fsig][keccak256(abi.encodePacked(ins, field_depth))] = true;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::79 => emit SlotFound(who, fsig, keccak256(abi.encodePacked(ins, field_depth)), uint256(reads[i]));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::80 => self.slots[who][fsig][keccak256(abi.encodePacked(ins, field_depth))] = uint256(reads[i]);
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::81 => self.finds[who][fsig][keccak256(abi.encodePacked(ins, field_depth))] = true;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::91 => require(self.finds[who][fsig][keccak256(abi.encodePacked(ins, field_depth))], "stdStorage find(StdStorage): Slot(s) not found.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::98 => return self.slots[who][fsig][keccak256(abi.encodePacked(ins, field_depth))];
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::190 => Vm private constant vm = Vm(address(uint160(uint256(keccak256("hevm cheat code")))));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::252 => if (!self.finds[who][fsig][keccak256(abi.encodePacked(ins, field_depth))]) {
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::255 => bytes32 slot = bytes32(self.slots[who][fsig][keccak256(abi.encodePacked(ins, field_depth))]);
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdUtils.sol::8 => VmSafe private constant vm = VmSafe(address(uint160(uint256(keccak256("hevm cheat code")))));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdUtils.sol::73 => if (nonce == 0x00) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd6), bytes1(0x94), deployer, bytes1(0x80))));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdUtils.sol::74 => if (nonce <= 0x7f) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd6), bytes1(0x94), deployer, uint8(nonce))));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdUtils.sol::77 => if (nonce <= 2 ** 8 - 1) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd7), bytes1(0x94), deployer, bytes1(0x81), uint8(nonce))));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdUtils.sol::78 => if (nonce <= 2 ** 16 - 1) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd8), bytes1(0x94), deployer, bytes1(0x82), uint16(nonce))));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdUtils.sol::79 => if (nonce <= 2 ** 24 - 1) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd9), bytes1(0x94), deployer, bytes1(0x83), uint24(nonce))));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdUtils.sol::87 => return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xda), bytes1(0x94), deployer, bytes1(0x84), uint32(nonce))));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdUtils.sol::91 => return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xff), deployer, salt, initcodeHash)));
2023-01-timeswap/packages/v2-pool/src/TimeswapV2PoolDeployer.sol::28 => poolPair = address(new TimeswapV2Pool{salt: keccak256(abi.encode(optionPair))}());
2023-01-timeswap/packages/v2-token/lib/forge-std/src/Base.sol::9 => address internal constant VM_ADDRESS = address(uint160(uint256(keccak256("hevm cheat code"))));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/Base.sol::13 => address internal constant DEFAULT_SENDER = address(uint160(uint256(keccak256("foundry default caller"))));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdAssertions.sol::53 => if (keccak256(abi.encode(a)) != keccak256(abi.encode(b))) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdAssertions.sol::62 => if (keccak256(abi.encode(a)) != keccak256(abi.encode(b))) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdAssertions.sol::71 => if (keccak256(abi.encode(a)) != keccak256(abi.encode(b))) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdAssertions.sol::80 => if (keccak256(abi.encode(a)) != keccak256(abi.encode(b))) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdAssertions.sol::87 => if (keccak256(abi.encode(a)) != keccak256(abi.encode(b))) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdAssertions.sol::94 => if (keccak256(abi.encode(a)) != keccak256(abi.encode(b))) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::39 => VmSafe private constant vm = VmSafe(address(uint160(uint256(keccak256("hevm cheat code")))));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::103 => bytes(foundAlias).length == 0 || keccak256(bytes(foundAlias)) == keccak256(bytes(chainAlias)),
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::143 => if (keccak256(notFoundError) != keccak256(err) || bytes(chain.rpcUrl).length == 0) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdCheats.sol::10 => Vm private constant vm = Vm(address(uint160(uint256(keccak256("hevm cheat code")))));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdCheats.sol::389 => privateKey = uint256(keccak256(abi.encodePacked(name)));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdCheats.sol::461 => Vm private constant vm = Vm(address(uint160(uint256(keccak256("hevm cheat code")))));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdJson.sol::30 => VmSafe private constant vm = VmSafe(address(uint160(uint256(keccak256("hevm cheat code")))));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::20 => Vm private constant vm = Vm(address(uint160(uint256(keccak256("hevm cheat code")))));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::23 => return bytes4(keccak256(bytes(sigStr)));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::39 => if (self.finds[who][fsig][keccak256(abi.encodePacked(ins, field_depth))]) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::40 => return self.slots[who][fsig][keccak256(abi.encodePacked(ins, field_depth))];
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::59 => emit SlotFound(who, fsig, keccak256(abi.encodePacked(ins, field_depth)), uint256(reads[0]));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::60 => self.slots[who][fsig][keccak256(abi.encodePacked(ins, field_depth))] = uint256(reads[0]);
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::61 => self.finds[who][fsig][keccak256(abi.encodePacked(ins, field_depth))] = true;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::79 => emit SlotFound(who, fsig, keccak256(abi.encodePacked(ins, field_depth)), uint256(reads[i]));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::80 => self.slots[who][fsig][keccak256(abi.encodePacked(ins, field_depth))] = uint256(reads[i]);
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::81 => self.finds[who][fsig][keccak256(abi.encodePacked(ins, field_depth))] = true;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::91 => require(self.finds[who][fsig][keccak256(abi.encodePacked(ins, field_depth))], "stdStorage find(StdStorage): Slot(s) not found.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::98 => return self.slots[who][fsig][keccak256(abi.encodePacked(ins, field_depth))];
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::190 => Vm private constant vm = Vm(address(uint160(uint256(keccak256("hevm cheat code")))));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::252 => if (!self.finds[who][fsig][keccak256(abi.encodePacked(ins, field_depth))]) {
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::255 => bytes32 slot = bytes32(self.slots[who][fsig][keccak256(abi.encodePacked(ins, field_depth))]);
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::16 => VmSafe private constant vm = VmSafe(address(uint160(uint256(keccak256("hevm cheat code")))));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::89 => if (nonce == 0x00) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd6), bytes1(0x94), deployer, bytes1(0x80))));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::90 => if (nonce <= 0x7f) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd6), bytes1(0x94), deployer, uint8(nonce))));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::93 => if (nonce <= 2 ** 8 - 1) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd7), bytes1(0x94), deployer, bytes1(0x81), uint8(nonce))));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::94 => if (nonce <= 2 ** 16 - 1) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd8), bytes1(0x94), deployer, bytes1(0x82), uint16(nonce))));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::95 => if (nonce <= 2 ** 24 - 1) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd9), bytes1(0x94), deployer, bytes1(0x83), uint24(nonce))));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::103 => return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xda), bytes1(0x94), deployer, bytes1(0x84), uint32(nonce))));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::107 => return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xff), deployer, salt, initcodeHash)));
2023-01-timeswap/packages/v2-token/src/TimeswapV2Token.sol::46 => bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
2023-01-timeswap/packages/v2-token/src/TimeswapV2Token.sol::53 => bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
2023-01-timeswap/packages/v2-token/src/TimeswapV2Token.sol::61 => bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
2023-01-timeswap/packages/v2-token/src/structs/Position.sol::35 => return keccak256(abi.encode(timeswapV2TokenPosition));
2023-01-timeswap/packages/v2-token/src/structs/Position.sol::40 => return keccak256(abi.encode(timeswapV2LiquidityTokenPosition));
```
#### Tools used
Manual VS code audit

### 5th BUG
Long Revert Strings

#### Impact
Issue Information: 
Shortening revert strings to fit in 32 bytes will decrease gas costs for deployment and gas costs when the revert condition has been met.

If the contract(s) in scope allow using Solidity >=0.8.4, consider using Custom Errors as they are more gas efficient while allowing developers to describe the error in detail using NatSpec.

Example
🤦 Bad:

require(condition, "UniswapV3: The reentrancy guard. A transaction cannot re-enter the pool mid-swap");
🚀 Good (with shorter string):

// TODO: Provide link to a reference of error codes
require(condition, "LOK");
🚀 Good (with custom errors):

/// @notice A transaction cannot re-enter the pool mid-swap.
error NoReentrancy();

// ...

if (!condition) {
    revert NoReentrancy();
}

#### Findings:
```
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdAssertions.sol::30 => emit log("Error: a == b not satisfied [bool]");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdAssertions.sol::54 => emit log("Error: a == b not satisfied [uint[]]");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdAssertions.sol::63 => emit log("Error: a == b not satisfied [int[]]");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdAssertions.sol::72 => emit log("Error: a == b not satisfied [address[]]");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdAssertions.sol::109 => emit log("Error: a ~= b not satisfied [uint]");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdAssertions.sol::131 => emit log("Error: a ~= b not satisfied [int]");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdAssertions.sol::159 => emit log("Error: a ~= b not satisfied [uint]");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdAssertions.sol::190 => emit log("Error: a ~= b not satisfied [int]");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::211 => stdChains["anvil"] = Chain("Anvil", 31337, "http://127.0.0.1:8545");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::212 => stdChains["hardhat"] = Chain("Hardhat", 31337, "http://127.0.0.1:8545");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::213 => stdChains["mainnet"] = Chain("Mainnet", 1, "https://mainnet.infura.io/v3/6770454bc6ea42c58aac12978531b93f");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::214 => stdChains["goerli"] = Chain("Goerli", 5, "https://goerli.infura.io/v3/6770454bc6ea42c58aac12978531b93f");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::215 => stdChains["sepolia"] = Chain("Sepolia", 11155111, "https://rpc.sepolia.dev");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::216 => stdChains["optimism"] = Chain("Optimism", 10, "https://mainnet.optimism.io");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::217 => stdChains["optimism_goerli"] = Chain("Optimism Goerli", 420, "https://goerli.optimism.io");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::218 => stdChains["arbitrum_one"] = Chain("Arbitrum One", 42161, "https://arb1.arbitrum.io/rpc");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::219 => stdChains["arbitrum_one_goerli"] = Chain("Arbitrum One Goerli", 421613, "https://goerli-rollup.arbitrum.io/rpc");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::220 => stdChains["arbitrum_nova"] = Chain("Arbitrum Nova", 42170, "https://nova.arbitrum.io/rpc");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::221 => stdChains["polygon"] = Chain("Polygon", 137, "https://polygon-rpc.com");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::222 => stdChains["polygon_mumbai"] = Chain("Polygon Mumbai", 80001, "https://rpc-mumbai.matic.today");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::223 => stdChains["avalanche"] = Chain("Avalanche", 43114, "https://api.avax.network/ext/bc/C/rpc");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::224 => stdChains["avalanche_fuji"] = Chain("Avalanche Fuji", 43113, "https://api.avax-test.network/ext/bc/C/rpc");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::225 => stdChains["bnb_smart_chain"] = Chain("BNB Smart Chain", 56, "https://bsc-dataseed1.binance.org");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::226 => stdChains["bnb_smart_chain_testnet"] = Chain("BNB Smart Chain Testnet", 97, "https://data-seed-prebsc-1-s1.binance.org:8545"); // forgefmt: disable-line
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::227 => stdChains["gnosis_chain"] = Chain("Gnosis Chain", 100, "https://rpc.gnosischain.com");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::254 => if (chainId == stdChains["optimism"].chainId || chainId == stdChains["optimism_goerli"].chainId) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::257 => } else if (chainId == stdChains["arbitrum_one"].chainId || chainId == stdChains["arbitrum_one_goerli"].chainId) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::260 => } else if (chainId == stdChains["avalanche"].chainId || chainId == stdChains["avalanche_fuji"].chainId) {
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::325 => string memory key = string(abi.encodePacked(".transactions[", vm.toString(index), "]"));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::341 => string memory key = string(abi.encodePacked(".receipts[", vm.toString(index), "]"));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::399 => require(addr != address(0), "StdCheats deployCode(string,bytes): Deployment failed.");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::409 => require(addr != address(0), "StdCheats deployCode(string): Deployment failed.");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::420 => require(addr != address(0), "StdCheats deployCode(string,bytes,uint256): Deployment failed.");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::430 => require(addr != address(0), "StdCheats deployCode(string,uint256): Deployment failed.");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::451 => require(b.length <= 32, "StdCheats _bytesToUint(bytes): Bytes length exceeds 32.");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::57 => require(false, "stdStorage find(StdStorage): Packed slot. This would cause dangerous overwriting and currently isn't supported.");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::88 => require(false, "stdStorage find(StdStorage): No storage use detected for target.");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::91 => require(self.finds[who][fsig][keccak256(abi.encodePacked(ins, field_depth))], "stdStorage find(StdStorage): Slot(s) not found.");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::150 => revert("stdStorage read_bool(StdStorage): Cannot decode. Make sure you are reading a bool.");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::265 => require(false, "stdStorage find(StdStorage): Packed slot. This would cause dangerous overwriting and currently isn't supported.");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdUtils.sol::10 => require(min <= max, "StdUtils bound(uint256,uint256,uint256): Max is less than min.");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdUtils.sol::70 => require(b.length <= 32, "StdUtils bytesToUint(bytes): Bytes length exceeds 32.");
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::762 => _sendLogPayload(abi.encodeWithSignature("log(uint,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::858 => _sendLogPayload(abi.encodeWithSignature("log(string,string,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::882 => _sendLogPayload(abi.encodeWithSignature("log(string,string,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::890 => _sendLogPayload(abi.encodeWithSignature("log(string,string,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::978 => _sendLogPayload(abi.encodeWithSignature("log(string,address,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::986 => _sendLogPayload(abi.encodeWithSignature("log(string,address,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::1010 => _sendLogPayload(abi.encodeWithSignature("log(string,address,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::1018 => _sendLogPayload(abi.encodeWithSignature("log(string,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::1274 => _sendLogPayload(abi.encodeWithSignature("log(bool,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::1338 => _sendLogPayload(abi.encodeWithSignature("log(address,uint,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::1362 => _sendLogPayload(abi.encodeWithSignature("log(address,string,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::1370 => _sendLogPayload(abi.encodeWithSignature("log(address,string,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::1394 => _sendLogPayload(abi.encodeWithSignature("log(address,string,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::1402 => _sendLogPayload(abi.encodeWithSignature("log(address,string,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::1466 => _sendLogPayload(abi.encodeWithSignature("log(address,bool,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::1482 => _sendLogPayload(abi.encodeWithSignature("log(address,address,uint,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::1490 => _sendLogPayload(abi.encodeWithSignature("log(address,address,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::1498 => _sendLogPayload(abi.encodeWithSignature("log(address,address,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::1514 => _sendLogPayload(abi.encodeWithSignature("log(address,address,bool,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::1518 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,uint)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::1522 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::1526 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::1530 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::515 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::519 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::523 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,uint256,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::527 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::531 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::535 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::543 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::547 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,bool,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::559 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,bool,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::563 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::567 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::571 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,address,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::575 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::579 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::583 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::591 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::595 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::599 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::607 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::627 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::631 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::639 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::643 => _sendLogPayload(abi.encodeWithSignature("log(uint256,bool,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::655 => _sendLogPayload(abi.encodeWithSignature("log(uint256,bool,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::691 => _sendLogPayload(abi.encodeWithSignature("log(uint256,bool,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::703 => _sendLogPayload(abi.encodeWithSignature("log(uint256,bool,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::707 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::711 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::715 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,uint256,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::719 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::723 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::727 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::735 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::739 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,bool,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::751 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,bool,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::755 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::759 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::763 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,address,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::767 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::771 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::775 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::783 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::787 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::791 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::799 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::819 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::823 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::831 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::835 => _sendLogPayload(abi.encodeWithSignature("log(string,string,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::839 => _sendLogPayload(abi.encodeWithSignature("log(string,string,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::847 => _sendLogPayload(abi.encodeWithSignature("log(string,string,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::851 => _sendLogPayload(abi.encodeWithSignature("log(string,string,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::863 => _sendLogPayload(abi.encodeWithSignature("log(string,string,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::883 => _sendLogPayload(abi.encodeWithSignature("log(string,string,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::887 => _sendLogPayload(abi.encodeWithSignature("log(string,string,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::895 => _sendLogPayload(abi.encodeWithSignature("log(string,string,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::963 => _sendLogPayload(abi.encodeWithSignature("log(string,address,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::967 => _sendLogPayload(abi.encodeWithSignature("log(string,address,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::975 => _sendLogPayload(abi.encodeWithSignature("log(string,address,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::979 => _sendLogPayload(abi.encodeWithSignature("log(string,address,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::983 => _sendLogPayload(abi.encodeWithSignature("log(string,address,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::991 => _sendLogPayload(abi.encodeWithSignature("log(string,address,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1011 => _sendLogPayload(abi.encodeWithSignature("log(string,address,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1015 => _sendLogPayload(abi.encodeWithSignature("log(string,address,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1023 => _sendLogPayload(abi.encodeWithSignature("log(string,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1027 => _sendLogPayload(abi.encodeWithSignature("log(bool,uint256,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1039 => _sendLogPayload(abi.encodeWithSignature("log(bool,uint256,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1075 => _sendLogPayload(abi.encodeWithSignature("log(bool,uint256,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1087 => _sendLogPayload(abi.encodeWithSignature("log(bool,uint256,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1219 => _sendLogPayload(abi.encodeWithSignature("log(bool,address,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1231 => _sendLogPayload(abi.encodeWithSignature("log(bool,address,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1267 => _sendLogPayload(abi.encodeWithSignature("log(bool,address,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1279 => _sendLogPayload(abi.encodeWithSignature("log(bool,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1283 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1287 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1291 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,uint256,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1295 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1299 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1303 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1311 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1315 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,bool,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1327 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,bool,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1331 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1335 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1339 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,address,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1343 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1347 => _sendLogPayload(abi.encodeWithSignature("log(address,string,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1351 => _sendLogPayload(abi.encodeWithSignature("log(address,string,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1359 => _sendLogPayload(abi.encodeWithSignature("log(address,string,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1363 => _sendLogPayload(abi.encodeWithSignature("log(address,string,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1367 => _sendLogPayload(abi.encodeWithSignature("log(address,string,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1375 => _sendLogPayload(abi.encodeWithSignature("log(address,string,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1395 => _sendLogPayload(abi.encodeWithSignature("log(address,string,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1399 => _sendLogPayload(abi.encodeWithSignature("log(address,string,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1407 => _sendLogPayload(abi.encodeWithSignature("log(address,string,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1411 => _sendLogPayload(abi.encodeWithSignature("log(address,bool,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1423 => _sendLogPayload(abi.encodeWithSignature("log(address,bool,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1459 => _sendLogPayload(abi.encodeWithSignature("log(address,bool,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1471 => _sendLogPayload(abi.encodeWithSignature("log(address,bool,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1475 => _sendLogPayload(abi.encodeWithSignature("log(address,address,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1479 => _sendLogPayload(abi.encodeWithSignature("log(address,address,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1483 => _sendLogPayload(abi.encodeWithSignature("log(address,address,uint256,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1487 => _sendLogPayload(abi.encodeWithSignature("log(address,address,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1491 => _sendLogPayload(abi.encodeWithSignature("log(address,address,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1495 => _sendLogPayload(abi.encodeWithSignature("log(address,address,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1503 => _sendLogPayload(abi.encodeWithSignature("log(address,address,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1507 => _sendLogPayload(abi.encodeWithSignature("log(address,address,bool,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1519 => _sendLogPayload(abi.encodeWithSignature("log(address,address,bool,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1523 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1527 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1531 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::1535 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-option/src/TimeswapV2Option.sol::4 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
2023-01-timeswap/packages/v2-option/src/TimeswapV2Option.sol::5 => import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
2023-01-timeswap/packages/v2-option/src/TimeswapV2Option.sol::12 => import {ITimeswapV2Option} from "./interfaces/ITimeswapV2Option.sol";
2023-01-timeswap/packages/v2-option/src/TimeswapV2Option.sol::13 => import {ITimeswapV2OptionDeployer} from "./interfaces/ITimeswapV2OptionDeployer.sol";
2023-01-timeswap/packages/v2-option/src/TimeswapV2Option.sol::14 => import {ITimeswapV2OptionMintCallback} from "./interfaces/callbacks/ITimeswapV2OptionMintCallback.sol";
2023-01-timeswap/packages/v2-option/src/TimeswapV2Option.sol::15 => import {ITimeswapV2OptionBurnCallback} from "./interfaces/callbacks/ITimeswapV2OptionBurnCallback.sol";
2023-01-timeswap/packages/v2-option/src/TimeswapV2Option.sol::16 => import {ITimeswapV2OptionSwapCallback} from "./interfaces/callbacks/ITimeswapV2OptionSwapCallback.sol";
2023-01-timeswap/packages/v2-option/src/TimeswapV2Option.sol::17 => import {ITimeswapV2OptionCollectCallback} from "./interfaces/callbacks/ITimeswapV2OptionCollectCallback.sol";
2023-01-timeswap/packages/v2-option/src/TimeswapV2OptionDeployer.sol::6 => import {ITimeswapV2OptionDeployer} from "./interfaces/ITimeswapV2OptionDeployer.sol";
2023-01-timeswap/packages/v2-option/src/TimeswapV2OptionFactory.sol::4 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
2023-01-timeswap/packages/v2-option/src/TimeswapV2OptionFactory.sol::5 => import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
2023-01-timeswap/packages/v2-option/src/TimeswapV2OptionFactory.sol::9 => import {ITimeswapV2OptionFactory} from "./interfaces/ITimeswapV2OptionFactory.sol";
2023-01-timeswap/packages/v2-option/src/libraries/OptionFactory.sol::8 => import {ITimeswapV2OptionFactory} from "../interfaces/ITimeswapV2OptionFactory.sol";
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdAssertions.sol::30 => emit log("Error: a == b not satisfied [bool]");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdAssertions.sol::54 => emit log("Error: a == b not satisfied [uint[]]");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdAssertions.sol::63 => emit log("Error: a == b not satisfied [int[]]");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdAssertions.sol::72 => emit log("Error: a == b not satisfied [address[]]");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdAssertions.sol::109 => emit log("Error: a ~= b not satisfied [uint]");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdAssertions.sol::131 => emit log("Error: a ~= b not satisfied [int]");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdAssertions.sol::159 => emit log("Error: a ~= b not satisfied [uint]");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdAssertions.sol::190 => emit log("Error: a ~= b not satisfied [int]");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::63 => require(bytes(chainAlias).length != 0, "StdChains getChain(string): Chain alias cannot be the empty string.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::73 => require(chainId != 0, "StdChains getChain(uint256): Chain ID cannot be 0.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::79 => require(chain.chainId != 0, string(abi.encodePacked("StdChains getChain(uint256): Chain with ID ", vm.toString(chainId), " not found.")));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::86 => require(bytes(chainAlias).length != 0, "StdChains setChain(string,Chain): Chain alias cannot be the empty string.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::88 => require(chain.chainId != 0, "StdChains setChain(string,Chain): Chain ID cannot be 0.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::95 => string(abi.encodePacked("StdChains setChain(string,Chain): Chain ID ", vm.toString(chain.chainId), ' already used by "', foundAlias, '".'))
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::114 => bytes memory notFoundError = abi.encodeWithSignature("CheatCodeError", string(abi.encodePacked("invalid rpc url ", chainAlias)));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::132 => setChainWithDefaultRpcUrl("anvil", Chain("Anvil", 31337, "http://127.0.0.1:8545"));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::133 => setChainWithDefaultRpcUrl("mainnet", Chain("Mainnet", 1, "https://mainnet.infura.io/v3/6770454bc6ea42c58aac12978531b93f"));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::134 => setChainWithDefaultRpcUrl("goerli", Chain("Goerli", 5, "https://goerli.infura.io/v3/6770454bc6ea42c58aac12978531b93f"));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::135 => setChainWithDefaultRpcUrl("sepolia", Chain("Sepolia", 11155111, "https://sepolia.infura.io/v3/6770454bc6ea42c58aac12978531b93f"));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::136 => setChainWithDefaultRpcUrl("optimism", Chain("Optimism", 10, "https://mainnet.optimism.io"));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::137 => setChainWithDefaultRpcUrl("optimism_goerli", Chain("Optimism Goerli", 420, "https://goerli.optimism.io"));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::138 => setChainWithDefaultRpcUrl("arbitrum_one", Chain("Arbitrum One", 42161, "https://arb1.arbitrum.io/rpc"));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::139 => setChainWithDefaultRpcUrl("arbitrum_one_goerli", Chain("Arbitrum One Goerli", 421613, "https://goerli-rollup.arbitrum.io/rpc"));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::140 => setChainWithDefaultRpcUrl("arbitrum_nova", Chain("Arbitrum Nova", 42170, "https://nova.arbitrum.io/rpc"));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::141 => setChainWithDefaultRpcUrl("polygon", Chain("Polygon", 137, "https://polygon-rpc.com"));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::142 => setChainWithDefaultRpcUrl("polygon_mumbai", Chain("Polygon Mumbai", 80001, "https://rpc-mumbai.maticvigil.com"));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::143 => setChainWithDefaultRpcUrl("avalanche", Chain("Avalanche", 43114, "https://api.avax.network/ext/bc/C/rpc"));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::144 => setChainWithDefaultRpcUrl("avalanche_fuji", Chain("Avalanche Fuji", 43113, "https://api.avax-test.network/ext/bc/C/rpc"));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::145 => setChainWithDefaultRpcUrl("bnb_smart_chain", Chain("BNB Smart Chain", 56, "https://bsc-dataseed1.binance.org"));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::146 => setChainWithDefaultRpcUrl("bnb_smart_chain_testnet", Chain("BNB Smart Chain Testnet", 97, "https://data-seed-prebsc-1-s1.binance.org:8545")); // forgefmt: disable-line
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::147 => setChainWithDefaultRpcUrl("gnosis_chain", Chain("Gnosis Chain", 100, "https://rpc.gnosischain.com"));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdCheats.sol::279 => string memory key = string(abi.encodePacked(".transactions[", vm.toString(index), "]"));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdCheats.sol::295 => string memory key = string(abi.encodePacked(".receipts[", vm.toString(index), "]"));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdCheats.sol::353 => require(addr != address(0), "StdCheats deployCode(string,bytes): Deployment failed.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdCheats.sol::363 => require(addr != address(0), "StdCheats deployCode(string): Deployment failed.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdCheats.sol::374 => require(addr != address(0), "StdCheats deployCode(string,bytes,uint256): Deployment failed.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdCheats.sol::384 => require(addr != address(0), "StdCheats deployCode(string,uint256): Deployment failed.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdCheats.sol::405 => require(b.length <= 32, "StdCheats _bytesToUint(bytes): Bytes length exceeds 32.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::57 => require(false, "stdStorage find(StdStorage): Packed slot. This would cause dangerous overwriting and currently isn't supported.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::88 => require(false, "stdStorage find(StdStorage): No storage use detected for target.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::91 => require(self.finds[who][fsig][keccak256(abi.encodePacked(ins, field_depth))], "stdStorage find(StdStorage): Slot(s) not found.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::150 => revert("stdStorage read_bool(StdStorage): Cannot decode. Make sure you are reading a bool.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::265 => require(false, "stdStorage find(StdStorage): Packed slot. This would cause dangerous overwriting and currently isn't supported.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdUtils.sol::15 => require(min <= max, "StdUtils bound(uint256,uint256,uint256): Max is less than min.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdUtils.sol::47 => require(min <= max, "StdUtils bound(int256,int256,int256): Max is less than min.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdUtils.sol::95 => require(b.length <= 32, "StdUtils bytesToUint(bytes): Bytes length exceeds 32.");
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::762 => _sendLogPayload(abi.encodeWithSignature("log(uint,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::858 => _sendLogPayload(abi.encodeWithSignature("log(string,string,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::882 => _sendLogPayload(abi.encodeWithSignature("log(string,string,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::890 => _sendLogPayload(abi.encodeWithSignature("log(string,string,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::978 => _sendLogPayload(abi.encodeWithSignature("log(string,address,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::986 => _sendLogPayload(abi.encodeWithSignature("log(string,address,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::1010 => _sendLogPayload(abi.encodeWithSignature("log(string,address,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::1018 => _sendLogPayload(abi.encodeWithSignature("log(string,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::1274 => _sendLogPayload(abi.encodeWithSignature("log(bool,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::1338 => _sendLogPayload(abi.encodeWithSignature("log(address,uint,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::1362 => _sendLogPayload(abi.encodeWithSignature("log(address,string,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::1370 => _sendLogPayload(abi.encodeWithSignature("log(address,string,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::1394 => _sendLogPayload(abi.encodeWithSignature("log(address,string,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::1402 => _sendLogPayload(abi.encodeWithSignature("log(address,string,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::1466 => _sendLogPayload(abi.encodeWithSignature("log(address,bool,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::1482 => _sendLogPayload(abi.encodeWithSignature("log(address,address,uint,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::1490 => _sendLogPayload(abi.encodeWithSignature("log(address,address,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::1498 => _sendLogPayload(abi.encodeWithSignature("log(address,address,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::1514 => _sendLogPayload(abi.encodeWithSignature("log(address,address,bool,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::1518 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,uint)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::1522 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::1526 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::1530 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::523 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::527 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::531 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,uint256,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::535 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::539 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::543 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::551 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::555 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,bool,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::567 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,bool,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::571 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::575 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::579 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,address,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::583 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::587 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::591 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::599 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::603 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::607 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::615 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::635 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::639 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::647 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::651 => _sendLogPayload(abi.encodeWithSignature("log(uint256,bool,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::663 => _sendLogPayload(abi.encodeWithSignature("log(uint256,bool,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::699 => _sendLogPayload(abi.encodeWithSignature("log(uint256,bool,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::711 => _sendLogPayload(abi.encodeWithSignature("log(uint256,bool,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::715 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::719 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::723 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,uint256,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::727 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::731 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::735 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::743 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::747 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,bool,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::759 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,bool,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::763 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::767 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::771 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,address,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::775 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::779 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::783 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::791 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::795 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::799 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::807 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::827 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::831 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::839 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::843 => _sendLogPayload(abi.encodeWithSignature("log(string,string,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::847 => _sendLogPayload(abi.encodeWithSignature("log(string,string,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::855 => _sendLogPayload(abi.encodeWithSignature("log(string,string,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::859 => _sendLogPayload(abi.encodeWithSignature("log(string,string,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::871 => _sendLogPayload(abi.encodeWithSignature("log(string,string,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::891 => _sendLogPayload(abi.encodeWithSignature("log(string,string,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::895 => _sendLogPayload(abi.encodeWithSignature("log(string,string,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::903 => _sendLogPayload(abi.encodeWithSignature("log(string,string,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::971 => _sendLogPayload(abi.encodeWithSignature("log(string,address,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::975 => _sendLogPayload(abi.encodeWithSignature("log(string,address,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::983 => _sendLogPayload(abi.encodeWithSignature("log(string,address,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::987 => _sendLogPayload(abi.encodeWithSignature("log(string,address,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::991 => _sendLogPayload(abi.encodeWithSignature("log(string,address,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::999 => _sendLogPayload(abi.encodeWithSignature("log(string,address,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1019 => _sendLogPayload(abi.encodeWithSignature("log(string,address,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1023 => _sendLogPayload(abi.encodeWithSignature("log(string,address,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1031 => _sendLogPayload(abi.encodeWithSignature("log(string,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1035 => _sendLogPayload(abi.encodeWithSignature("log(bool,uint256,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1047 => _sendLogPayload(abi.encodeWithSignature("log(bool,uint256,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1083 => _sendLogPayload(abi.encodeWithSignature("log(bool,uint256,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1095 => _sendLogPayload(abi.encodeWithSignature("log(bool,uint256,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1227 => _sendLogPayload(abi.encodeWithSignature("log(bool,address,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1239 => _sendLogPayload(abi.encodeWithSignature("log(bool,address,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1275 => _sendLogPayload(abi.encodeWithSignature("log(bool,address,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1287 => _sendLogPayload(abi.encodeWithSignature("log(bool,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1291 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1295 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1299 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,uint256,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1303 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1307 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1311 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1319 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1323 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,bool,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1335 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,bool,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1339 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1343 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1347 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,address,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1351 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1355 => _sendLogPayload(abi.encodeWithSignature("log(address,string,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1359 => _sendLogPayload(abi.encodeWithSignature("log(address,string,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1367 => _sendLogPayload(abi.encodeWithSignature("log(address,string,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1371 => _sendLogPayload(abi.encodeWithSignature("log(address,string,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1375 => _sendLogPayload(abi.encodeWithSignature("log(address,string,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1383 => _sendLogPayload(abi.encodeWithSignature("log(address,string,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1403 => _sendLogPayload(abi.encodeWithSignature("log(address,string,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1407 => _sendLogPayload(abi.encodeWithSignature("log(address,string,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1415 => _sendLogPayload(abi.encodeWithSignature("log(address,string,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1419 => _sendLogPayload(abi.encodeWithSignature("log(address,bool,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1431 => _sendLogPayload(abi.encodeWithSignature("log(address,bool,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1467 => _sendLogPayload(abi.encodeWithSignature("log(address,bool,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1479 => _sendLogPayload(abi.encodeWithSignature("log(address,bool,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1483 => _sendLogPayload(abi.encodeWithSignature("log(address,address,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1487 => _sendLogPayload(abi.encodeWithSignature("log(address,address,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1491 => _sendLogPayload(abi.encodeWithSignature("log(address,address,uint256,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1495 => _sendLogPayload(abi.encodeWithSignature("log(address,address,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1499 => _sendLogPayload(abi.encodeWithSignature("log(address,address,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1503 => _sendLogPayload(abi.encodeWithSignature("log(address,address,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1511 => _sendLogPayload(abi.encodeWithSignature("log(address,address,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1515 => _sendLogPayload(abi.encodeWithSignature("log(address,address,bool,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1527 => _sendLogPayload(abi.encodeWithSignature("log(address,address,bool,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1531 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1535 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1539 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::1543 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-pool/src/TimeswapV2Pool.sol::17 => import {ITimeswapV2PoolFactory} from "./interfaces/ITimeswapV2PoolFactory.sol";
2023-01-timeswap/packages/v2-pool/src/TimeswapV2Pool.sol::18 => import {ITimeswapV2PoolDeployer} from "./interfaces/ITimeswapV2PoolDeployer.sol";
2023-01-timeswap/packages/v2-pool/src/TimeswapV2Pool.sol::20 => import {ITimeswapV2PoolMintCallback} from "./interfaces/callbacks/ITimeswapV2PoolMintCallback.sol";
2023-01-timeswap/packages/v2-pool/src/TimeswapV2Pool.sol::21 => import {ITimeswapV2PoolBurnCallback, ITimeswapV2PoolBurn2Callback} from "./interfaces/callbacks/ITimeswapV2PoolBurnCallback.sol";
2023-01-timeswap/packages/v2-pool/src/TimeswapV2Pool.sol::22 => import {ITimeswapV2PoolDeleverageCallback} from "./interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol";
2023-01-timeswap/packages/v2-pool/src/TimeswapV2Pool.sol::23 => import {ITimeswapV2PoolLeverageCallback} from "./interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol";
2023-01-timeswap/packages/v2-pool/src/TimeswapV2Pool.sol::24 => import {ITimeswapV2PoolRebalanceCallback} from "./interfaces/callbacks/ITimeswapV2PoolRebalanceCallback.sol";
2023-01-timeswap/packages/v2-pool/src/TimeswapV2PoolDeployer.sol::6 => import {ITimeswapV2PoolDeployer} from "./interfaces/ITimeswapV2PoolDeployer.sol";
2023-01-timeswap/packages/v2-pool/src/TimeswapV2PoolFactory.sol::8 => import {ITimeswapV2PoolFactory} from "./interfaces/ITimeswapV2PoolFactory.sol";
2023-01-timeswap/packages/v2-pool/src/base/OwnableTwoSteps.sol::7 => import {IOwnableTwoSteps} from "../interfaces/IOwnableTwoSteps.sol";
2023-01-timeswap/packages/v2-pool/src/libraries/FeeCalculation.sol::5 => import {FullMath} from "@timeswap-labs/v2-library/src/FullMath.sol";
2023-01-timeswap/packages/v2-pool/src/libraries/PoolFactory.sol::6 => import {ITimeswapV2PoolFactory} from "../interfaces/ITimeswapV2PoolFactory.sol";
2023-01-timeswap/packages/v2-pool/src/structs/Pool.sol::12 => import {DurationCalculation} from "../libraries/DurationCalculation.sol";
2023-01-timeswap/packages/v2-pool/src/structs/Pool.sol::18 => import {ITimeswapV2PoolMintCallback} from "../interfaces/callbacks/ITimeswapV2PoolMintCallback.sol";
2023-01-timeswap/packages/v2-pool/src/structs/Pool.sol::19 => import {ITimeswapV2PoolBurnCallback} from "../interfaces/callbacks/ITimeswapV2PoolBurnCallback.sol";
2023-01-timeswap/packages/v2-pool/src/structs/Pool.sol::20 => import {ITimeswapV2PoolDeleverageCallback} from "../interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol";
2023-01-timeswap/packages/v2-pool/src/structs/Pool.sol::21 => import {ITimeswapV2PoolLeverageCallback} from "../interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol";
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdAssertions.sol::30 => emit log("Error: a == b not satisfied [bool]");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdAssertions.sol::54 => emit log("Error: a == b not satisfied [uint[]]");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdAssertions.sol::63 => emit log("Error: a == b not satisfied [int[]]");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdAssertions.sol::72 => emit log("Error: a == b not satisfied [address[]]");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdAssertions.sol::109 => emit log("Error: a ~= b not satisfied [uint]");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdAssertions.sol::131 => emit log("Error: a ~= b not satisfied [uint]");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdAssertions.sol::153 => emit log("Error: a ~= b not satisfied [int]");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdAssertions.sol::175 => emit log("Error: a ~= b not satisfied [int]");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdAssertions.sol::203 => emit log("Error: a ~= b not satisfied [uint]");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdAssertions.sol::239 => emit log("Error: a ~= b not satisfied [uint]");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdAssertions.sol::271 => emit log("Error: a ~= b not satisfied [int]");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdAssertions.sol::297 => emit log("Error: a ~= b not satisfied [int]");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::72 => require(bytes(chainAlias).length != 0, "StdChains getChain(string): Chain alias cannot be the empty string.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::82 => require(chainId != 0, "StdChains getChain(uint256): Chain ID cannot be 0.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::88 => require(chain.chainId != 0, string(abi.encodePacked("StdChains getChain(uint256): Chain with ID ", vm.toString(chainId), " not found.")));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::95 => require(bytes(chainAlias).length != 0, "StdChains setChain(string,ChainData): Chain alias cannot be the empty string.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::97 => require(chain.chainId != 0, "StdChains setChain(string,ChainData): Chain ID cannot be 0.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::104 => string(abi.encodePacked("StdChains setChain(string,ChainData): Chain ID ", vm.toString(chain.chainId), ' already used by "', foundAlias, '".'))
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::142 => bytes memory notFoundError = abi.encodeWithSignature("CheatCodeError", string(abi.encodePacked("invalid rpc url ", chainAlias)));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::160 => setChainWithDefaultRpcUrl("anvil", ChainData("Anvil", 31337, "http://127.0.0.1:8545"));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::161 => setChainWithDefaultRpcUrl("mainnet", ChainData("Mainnet", 1, "https://mainnet.infura.io/v3/6770454bc6ea42c58aac12978531b93f"));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::162 => setChainWithDefaultRpcUrl("goerli", ChainData("Goerli", 5, "https://goerli.infura.io/v3/6770454bc6ea42c58aac12978531b93f"));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::163 => setChainWithDefaultRpcUrl("sepolia", ChainData("Sepolia", 11155111, "https://sepolia.infura.io/v3/6770454bc6ea42c58aac12978531b93f"));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::164 => setChainWithDefaultRpcUrl("optimism", ChainData("Optimism", 10, "https://mainnet.optimism.io"));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::165 => setChainWithDefaultRpcUrl("optimism_goerli", ChainData("Optimism Goerli", 420, "https://goerli.optimism.io"));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::166 => setChainWithDefaultRpcUrl("arbitrum_one", ChainData("Arbitrum One", 42161, "https://arb1.arbitrum.io/rpc"));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::167 => setChainWithDefaultRpcUrl("arbitrum_one_goerli", ChainData("Arbitrum One Goerli", 421613, "https://goerli-rollup.arbitrum.io/rpc"));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::168 => setChainWithDefaultRpcUrl("arbitrum_nova", ChainData("Arbitrum Nova", 42170, "https://nova.arbitrum.io/rpc"));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::169 => setChainWithDefaultRpcUrl("polygon", ChainData("Polygon", 137, "https://polygon-rpc.com"));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::170 => setChainWithDefaultRpcUrl("polygon_mumbai", ChainData("Polygon Mumbai", 80001, "https://rpc-mumbai.maticvigil.com"));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::171 => setChainWithDefaultRpcUrl("avalanche", ChainData("Avalanche", 43114, "https://api.avax.network/ext/bc/C/rpc"));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::172 => setChainWithDefaultRpcUrl("avalanche_fuji", ChainData("Avalanche Fuji", 43113, "https://api.avax-test.network/ext/bc/C/rpc"));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::173 => setChainWithDefaultRpcUrl("bnb_smart_chain", ChainData("BNB Smart Chain", 56, "https://bsc-dataseed1.binance.org"));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::174 => setChainWithDefaultRpcUrl("bnb_smart_chain_testnet", ChainData("BNB Smart Chain Testnet", 97, "https://rpc.ankr.com/bsc_testnet_chapel"));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::175 => setChainWithDefaultRpcUrl("gnosis_chain", ChainData("Gnosis Chain", 100, "https://rpc.gnosischain.com"));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdCheats.sol::279 => string memory key = string(abi.encodePacked(".transactions[", vm.toString(index), "]"));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdCheats.sol::295 => string memory key = string(abi.encodePacked(".receipts[", vm.toString(index), "]"));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdCheats.sol::353 => require(addr != address(0), "StdCheats deployCode(string,bytes): Deployment failed.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdCheats.sol::363 => require(addr != address(0), "StdCheats deployCode(string): Deployment failed.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdCheats.sol::374 => require(addr != address(0), "StdCheats deployCode(string,bytes,uint256): Deployment failed.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdCheats.sol::384 => require(addr != address(0), "StdCheats deployCode(string,uint256): Deployment failed.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdCheats.sol::405 => require(b.length <= 32, "StdCheats _bytesToUint(bytes): Bytes length exceeds 32.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::57 => require(false, "stdStorage find(StdStorage): Packed slot. This would cause dangerous overwriting and currently isn't supported.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::88 => revert("stdStorage find(StdStorage): No storage use detected for target.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::91 => require(self.finds[who][fsig][keccak256(abi.encodePacked(ins, field_depth))], "stdStorage find(StdStorage): Slot(s) not found.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::150 => revert("stdStorage read_bool(StdStorage): Cannot decode. Make sure you are reading a bool.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::265 => require(false, "stdStorage find(StdStorage): Packed slot. This would cause dangerous overwriting and currently isn't supported.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::26 => require(min <= max, "StdUtils bound(uint256,uint256,uint256): Max is less than min.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::58 => require(min <= max, "StdUtils bound(int256,int256,int256): Max is less than min.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::79 => require(b.length <= 32, "StdUtils bytesToUint(bytes): Bytes length exceeds 32.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::116 => require(tokenCodeSize > 0, "StdUtils getTokenBalances(address,address[]): Token address is not a contract.");
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::762 => _sendLogPayload(abi.encodeWithSignature("log(uint,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::858 => _sendLogPayload(abi.encodeWithSignature("log(string,string,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::882 => _sendLogPayload(abi.encodeWithSignature("log(string,string,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::890 => _sendLogPayload(abi.encodeWithSignature("log(string,string,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::978 => _sendLogPayload(abi.encodeWithSignature("log(string,address,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::986 => _sendLogPayload(abi.encodeWithSignature("log(string,address,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::1010 => _sendLogPayload(abi.encodeWithSignature("log(string,address,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::1018 => _sendLogPayload(abi.encodeWithSignature("log(string,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::1274 => _sendLogPayload(abi.encodeWithSignature("log(bool,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::1338 => _sendLogPayload(abi.encodeWithSignature("log(address,uint,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::1362 => _sendLogPayload(abi.encodeWithSignature("log(address,string,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::1370 => _sendLogPayload(abi.encodeWithSignature("log(address,string,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::1394 => _sendLogPayload(abi.encodeWithSignature("log(address,string,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::1402 => _sendLogPayload(abi.encodeWithSignature("log(address,string,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::1466 => _sendLogPayload(abi.encodeWithSignature("log(address,bool,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::1482 => _sendLogPayload(abi.encodeWithSignature("log(address,address,uint,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::1490 => _sendLogPayload(abi.encodeWithSignature("log(address,address,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::1498 => _sendLogPayload(abi.encodeWithSignature("log(address,address,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::1514 => _sendLogPayload(abi.encodeWithSignature("log(address,address,bool,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::1518 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,uint)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::1522 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::1526 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::1530 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::523 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::527 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::531 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,uint256,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::535 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::539 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::543 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::551 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::555 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,bool,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::567 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,bool,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::571 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::575 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::579 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,address,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::583 => _sendLogPayload(abi.encodeWithSignature("log(uint256,uint256,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::587 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::591 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::599 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::603 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::607 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::615 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::635 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::639 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::647 => _sendLogPayload(abi.encodeWithSignature("log(uint256,string,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::651 => _sendLogPayload(abi.encodeWithSignature("log(uint256,bool,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::663 => _sendLogPayload(abi.encodeWithSignature("log(uint256,bool,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::699 => _sendLogPayload(abi.encodeWithSignature("log(uint256,bool,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::711 => _sendLogPayload(abi.encodeWithSignature("log(uint256,bool,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::715 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::719 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::723 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,uint256,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::727 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::731 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::735 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::743 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::747 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,bool,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::759 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,bool,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::763 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::767 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::771 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,address,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::775 => _sendLogPayload(abi.encodeWithSignature("log(uint256,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::779 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::783 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::791 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::795 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::799 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::807 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::827 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::831 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::839 => _sendLogPayload(abi.encodeWithSignature("log(string,uint256,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::843 => _sendLogPayload(abi.encodeWithSignature("log(string,string,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::847 => _sendLogPayload(abi.encodeWithSignature("log(string,string,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::855 => _sendLogPayload(abi.encodeWithSignature("log(string,string,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::859 => _sendLogPayload(abi.encodeWithSignature("log(string,string,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::871 => _sendLogPayload(abi.encodeWithSignature("log(string,string,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::891 => _sendLogPayload(abi.encodeWithSignature("log(string,string,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::895 => _sendLogPayload(abi.encodeWithSignature("log(string,string,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::903 => _sendLogPayload(abi.encodeWithSignature("log(string,string,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::971 => _sendLogPayload(abi.encodeWithSignature("log(string,address,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::975 => _sendLogPayload(abi.encodeWithSignature("log(string,address,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::983 => _sendLogPayload(abi.encodeWithSignature("log(string,address,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::987 => _sendLogPayload(abi.encodeWithSignature("log(string,address,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::991 => _sendLogPayload(abi.encodeWithSignature("log(string,address,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::999 => _sendLogPayload(abi.encodeWithSignature("log(string,address,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1019 => _sendLogPayload(abi.encodeWithSignature("log(string,address,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1023 => _sendLogPayload(abi.encodeWithSignature("log(string,address,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1031 => _sendLogPayload(abi.encodeWithSignature("log(string,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1035 => _sendLogPayload(abi.encodeWithSignature("log(bool,uint256,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1047 => _sendLogPayload(abi.encodeWithSignature("log(bool,uint256,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1083 => _sendLogPayload(abi.encodeWithSignature("log(bool,uint256,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1095 => _sendLogPayload(abi.encodeWithSignature("log(bool,uint256,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1227 => _sendLogPayload(abi.encodeWithSignature("log(bool,address,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1239 => _sendLogPayload(abi.encodeWithSignature("log(bool,address,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1275 => _sendLogPayload(abi.encodeWithSignature("log(bool,address,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1287 => _sendLogPayload(abi.encodeWithSignature("log(bool,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1291 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1295 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1299 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,uint256,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1303 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1307 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1311 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1319 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1323 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,bool,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1335 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,bool,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1339 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1343 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1347 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,address,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1351 => _sendLogPayload(abi.encodeWithSignature("log(address,uint256,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1355 => _sendLogPayload(abi.encodeWithSignature("log(address,string,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1359 => _sendLogPayload(abi.encodeWithSignature("log(address,string,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1367 => _sendLogPayload(abi.encodeWithSignature("log(address,string,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1371 => _sendLogPayload(abi.encodeWithSignature("log(address,string,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1375 => _sendLogPayload(abi.encodeWithSignature("log(address,string,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1383 => _sendLogPayload(abi.encodeWithSignature("log(address,string,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1403 => _sendLogPayload(abi.encodeWithSignature("log(address,string,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1407 => _sendLogPayload(abi.encodeWithSignature("log(address,string,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1415 => _sendLogPayload(abi.encodeWithSignature("log(address,string,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1419 => _sendLogPayload(abi.encodeWithSignature("log(address,bool,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1431 => _sendLogPayload(abi.encodeWithSignature("log(address,bool,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1467 => _sendLogPayload(abi.encodeWithSignature("log(address,bool,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1479 => _sendLogPayload(abi.encodeWithSignature("log(address,bool,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1483 => _sendLogPayload(abi.encodeWithSignature("log(address,address,uint256,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1487 => _sendLogPayload(abi.encodeWithSignature("log(address,address,uint256,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1491 => _sendLogPayload(abi.encodeWithSignature("log(address,address,uint256,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1495 => _sendLogPayload(abi.encodeWithSignature("log(address,address,uint256,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1499 => _sendLogPayload(abi.encodeWithSignature("log(address,address,string,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1503 => _sendLogPayload(abi.encodeWithSignature("log(address,address,string,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1511 => _sendLogPayload(abi.encodeWithSignature("log(address,address,string,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1515 => _sendLogPayload(abi.encodeWithSignature("log(address,address,bool,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1527 => _sendLogPayload(abi.encodeWithSignature("log(address,address,bool,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1531 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,uint256)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1535 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,string)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1539 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,bool)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::1543 => _sendLogPayload(abi.encodeWithSignature("log(address,address,address,address)", p0, p1, p2, p3));
2023-01-timeswap/packages/v2-token/src/TimeswapV2LiquidityToken.sol::4 => import {ERC1155} from "@openzeppelin/contracts/token/ERC1155/ERC1155.sol";
2023-01-timeswap/packages/v2-token/src/TimeswapV2LiquidityToken.sol::11 => import {ITimeswapV2LiquidityToken} from "./interfaces/ITimeswapV2LiquidityToken.sol";
2023-01-timeswap/packages/v2-token/src/TimeswapV2LiquidityToken.sol::13 => import {ITimeswapV2LiquidityTokenMintCallback} from "./interfaces/callbacks/ITimeswapV2LiquidityTokenMintCallback.sol";
2023-01-timeswap/packages/v2-token/src/TimeswapV2LiquidityToken.sol::14 => import {ITimeswapV2LiquidityTokenBurnCallback} from "./interfaces/callbacks/ITimeswapV2LiquidityTokenBurnCallback.sol";
2023-01-timeswap/packages/v2-token/src/TimeswapV2LiquidityToken.sol::15 => import {ITimeswapV2LiquidityTokenCollectCallback} from "./interfaces/callbacks/ITimeswapV2LiquidityTokenCollectCallback.sol";
2023-01-timeswap/packages/v2-token/src/TimeswapV2Token.sol::4 => import {ERC1155} from "@openzeppelin/contracts/token/ERC1155/ERC1155.sol";
2023-01-timeswap/packages/v2-token/src/TimeswapV2Token.sol::7 => 
2023-01-timeswap/packages/v2-token/src/TimeswapV2Token.sol::9 => import {OptionFactoryLibrary} from "@timeswap-labs/v2-option/src/libraries/OptionFactory.sol";
2023-01-timeswap/packages/v2-token/src/TimeswapV2Token.sol::10 => import {ReentrancyGuard} from "@timeswap-labs/v2-pool/src/libraries/ReentrancyGuard.sol";
2023-01-timeswap/packages/v2-token/src/TimeswapV2Token.sol::12 => 
2023-01-timeswap/packages/v2-token/src/TimeswapV2Token.sol::14 => import {ITimeswapV2Token} from "./interfaces/ITimeswapV2Token.sol";
2023-01-timeswap/packages/v2-token/src/TimeswapV2Token.sol::16 => import {ITimeswapV2TokenMintCallback} from "./interfaces/callbacks/ITimeswapV2TokenMintCallback.sol";
2023-01-timeswap/packages/v2-token/src/TimeswapV2Token.sol::17 => import {ITimeswapV2TokenBurnCallback} from "./interfaces/callbacks/ITimeswapV2TokenBurnCallback.sol";
2023-01-timeswap/packages/v2-token/src/TimeswapV2Token.sol::24 => 
2023-01-timeswap/packages/v2-token/src/TimeswapV2Token.sol::109 => console.log("reaches right before mint in timeswapv2Tokne::mint");
2023-01-timeswap/packages/v2-token/src/base/ERC1155Enumerable.sol::6 => import {ERC1155} from "@openzeppelin/contracts/token/ERC1155/ERC1155.sol";
2023-01-timeswap/packages/v2-token/src/base/ERC1155Enumerable.sol::8 => import {IERC1155Enumerable} from "../interfaces/IERC1155Enumerable.sol";
2023-01-timeswap/packages/v2-token/src/interfaces/IERC1155Enumerable.sol::6 => import "@openzeppelin/contracts/token/ERC1155/IERC1155.sol";
2023-01-timeswap/packages/v2-token/src/interfaces/ITimeswapV2Token.sol::4 => import {IERC1155} from "@openzeppelin/contracts/token/ERC1155/IERC1155.sol";
```
#### Tools used
Manual VS code audit

### 6th BUG
Use Shift Right/Left instead of Division/Multiplication if possible

#### Impact
Issue Information: 
A division/multiplication by any number x being a power of 2 can be calculated by shifting log2(x) to the right/left.

While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

Example
🤦 Bad:

uint256 b = a / 2;
uint256 c = a / 4;
uint256 d = a * 8;
🚀 Good:

uint256 b = a >> 1;
uint256 c = a >> 2;
uint256 d = a << 3;

#### Findings:
```
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::170 => out |= bytes32(b[offset + i] & 0xFF) >> (i * 8);
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::300 => out |= bytes32(b[offset + i] & 0xFF) >> (i * 8);
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdUtils.sol::52 => if (nonce <= 2 ** 8 - 1) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd7), bytes1(0x94), deployer, bytes1(0x81), uint8(nonce))));
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdUtils.sol::54 => if (nonce <= 2 ** 24 - 1) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd9), bytes1(0x94), deployer, bytes1(0x83), uint24(nonce))));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::170 => out |= bytes32(b[offset + i] & 0xFF) >> (i * 8);
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::300 => out |= bytes32(b[offset + i] & 0xFF) >> (i * 8);
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdUtils.sol::77 => if (nonce <= 2 ** 8 - 1) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd7), bytes1(0x94), deployer, bytes1(0x81), uint8(nonce))));
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdUtils.sol::79 => if (nonce <= 2 ** 24 - 1) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd9), bytes1(0x94), deployer, bytes1(0x83), uint24(nonce))));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::170 => out |= bytes32(b[offset + i] & 0xFF) >> (i * 8);
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::300 => out |= bytes32(b[offset + i] & 0xFF) >> (i * 8);
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::93 => if (nonce <= 2 ** 8 - 1) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd7), bytes1(0x94), deployer, bytes1(0x81), uint8(nonce))));
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::95 => if (nonce <= 2 ** 24 - 1) return addressFromLast20Bytes(keccak256(abi.encodePacked(bytes1(0xd9), bytes1(0x94), deployer, bytes1(0x83), uint24(nonce))));
```
#### Tools used
Manual VS code audit