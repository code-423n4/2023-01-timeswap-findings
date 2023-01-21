## [G-01] Make function `external` instead of `public`

### Description

Version 0.6.9 removed the restriction on public functions accepting calldata arguments. Public functions for Solidity versions 0.6.9 have to transfer the parameters to memory.

Note: valid only for solidity versions `<0.6.9`

### Findings

- https://github.com/code-423n4/2023-01-timeswap/blob/main//packages/v2-library/lib/forge-std/lib/ds-test/demo/demo.sol `pragma solidity >=0.5.0;`
  ```
  ::7 =>     function test_this() public pure {
  ::11 =>     function test_logs() public {
  ::42 =>     function test_old_logs() public {
  ::47 =>     function test_trace() public view {
  ::51 =>     function test_multiline() public {
  ::62 =>     function echo(string memory s1, string memory s2) public pure returns (string memory, string memory) {
  ::66 =>     function prove_this(uint x) public {
  ::71 =>     function test_logn() public {
  ::82 =>     function test_events() public {
  ::86 =>     function test_asserts() public {
  ::223 =>     function setUp() public {}
  ::225 =>     function test_pass() public pure {}
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main//packages/v2-library/lib/forge-std/lib/ds-test/src/test.sol `pragma solidity >=0.5.0;`
  ```
  ::50 =>     function failed() public returns (bool) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main//packages/v2-pool/lib/forge-std/lib/ds-test/demo/demo.sol `pragma solidity >=0.5.0;`
  ```
  ::7 =>     function test_this() public pure {
  ::11 =>     function test_logs() public {
  ::42 =>     function test_old_logs() public {
  ::47 =>     function test_trace() public view {
  ::51 =>     function test_multiline() public {
  ::62 =>     function echo(string memory s1, string memory s2) public pure returns (string memory, string memory) {
  ::66 =>     function prove_this(uint x) public {
  ::71 =>     function test_logn() public {
  ::82 =>     function test_events() public {
  ::86 =>     function test_asserts() public {
  ::223 =>     function setUp() public {}
  ::225 =>     function test_pass() public pure {}
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main//packages/v2-pool/lib/forge-std/lib/ds-test/src/test.sol `pragma solidity >=0.5.0;`
  ```
  ::50 =>     function failed() public returns (bool) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main//packages/v2-token/lib/forge-std/lib/ds-test/demo/demo.sol `pragma solidity >=0.5.0;`
  ```
  ::7 =>     function test_this() public pure {
  ::11 =>     function test_logs() public {
  ::42 =>     function test_old_logs() public {
  ::47 =>     function test_trace() public view {
  ::51 =>     function test_multiline() public {
  ::62 =>     function echo(string memory s1, string memory s2) public pure returns (string memory, string memory) {
  ::66 =>     function prove_this(uint x) public {
  ::71 =>     function test_logn() public {
  ::82 =>     function test_events() public {
  ::86 =>     function test_asserts() public {
  ::223 =>     function setUp() public {}
  ::225 =>     function test_pass() public pure {}
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main//packages/v2-token/lib/forge-std/lib/ds-test/src/test.sol `pragma solidity >=0.5.0;`
  ```
  ::50 =>     function failed() public returns (bool) {
  ```

### Resources

- [G009 - Make Function external instead of public](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md/#g009---make-function-external-instead-of-public)
- [Public vs External Functions in Solidity | Gustavo (Gus) Guimaraes post](https://gus-tavo-guim.medium.com/public-vs-external-functions-in-solidity-b46bcf0ba3ac)
- [StackOverflow answer](https://ethereum.stackexchange.com/questions/19380/external-vs-public-best-practices?answertab=active#tab-top)



## [G-02] Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

### Description

Gas consumption can be greater if you use items that are less than 32 bytes in size. This is such that the EVM can only handle 32 bytes at once. In order to increase the element's size from 32 bytes to the necessary amount, the EVM must do extra operations if it is lower than that. When necessary, it is advised to utilize a bigger size and then downcast.

### Findings

- https://github.com/code-423n4/2023-01-timeswap/blob/main//packages/v2-pool/src/TimeswapV2Pool.sol
  ```
  ::52 =>     mapping(uint256 => mapping(uint256 => uint96)) private reentrancyGuards;
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main//packages/v2-pool/src/libraries/ReentrancyGuard.sol
  ```
  ::12 =>     uint96 internal constant NOT_INTERACTED = 0;
  ::15 =>     uint96 internal constant NOT_ENTERED = 1;
  ::18 =>     uint96 internal constant ENTERED = 2;
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main//packages/v2-token/src/TimeswapV2LiquidityToken.sol
  ```
  ::41 =>     mapping(bytes32 => uint96) private reentrancyGuards;
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main//packages/v2-token/src/TimeswapV2Token.sol
  ```
  ::36 =>     mapping(bytes32 => uint96) private reentrancyGuards;
  ```

### References

- [Layout of State Variables in Storage | Solidity docs](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html#layout-of-state-variables-in-storage)
- [GAS OPTIMIZATIONS ISSUES by Gokberk Gulgun](https://hackmd.io/@W1m6lTsFT5WAy9C_lRTX_g/rkr5Laoys)