[L-01] Unspecific compiler version pragma
Proof Of Concept
2: pragma solidity =0.8.8;
01: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/BytesLib.sol
02: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/SafeCast.sol
02: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol
03: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Error.sol
04: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol
05: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Ownership.sol
06: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/StrikeConversion.sol
07: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/CatchError.sol
08: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol
09: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolDeployer.sol
10: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol
11: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/NoDelegateCall.sol
12: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/IOwnableTwoSteps.sol
13: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol
14: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2PoolDeployer.sol
15: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol
16: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolBurnCallback.sol
17: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolRebalanceCallback.sol
18: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol
19: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol
20: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol
21: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol
22: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/CallbackParam.sol
23: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol
24: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol
25: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol
26: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol
27: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/FeeCalculation.sol
28: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/Duration.sol
29: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/PoolFactory.sol
30: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantSum.sol
31: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/DurationCalculation.sol#L43
32: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/DurationWeight.sol
33: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/Fee.sol
34: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/PoolPair.sol
35: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ReentrancyGuard.sol
36: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/enums/Transaction.sol
37: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/ITimeswapV2Token.sol
38: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol
39: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/callbacks/ITimeswapV2LiquidityTokenMintCallback.sol
40: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/callbacks/ITimeswapV2TokenMintCallback.sol
41: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L2
42: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2OptionDeployer.sol#L16
43: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2OptionFactory.sol
44: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionSwapCallback.sol
45: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionMintCallback.sol
46: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionCollectCallback.sol
47: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionBurnCallback.sol
48: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionDeployer.sol
50: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Process.sol
51: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/CallbackParam.sol
52: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/StrikeAndMaturity.sol
53: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol
54: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol
55: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/NoDelegateCall.sol
56: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol
57: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/libraries/Proportion.sol
58: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/libraries/OptionPair.sol
59: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/libraries/OptionFactory.sol
60: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/enums/Transaction.sol
61: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/enums/Position.sol
62: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol

[L-02] abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()
Proof Of Concept
28: poolPair = address(new TimeswapV2Pool{salt: keccak256(abi.encode(optionPair))}());
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2PoolDeployer.sol#L28
46: bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
53:  bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
61: bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L46 
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L53
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L61
35: return keccak256(abi.encode(timeswapV2TokenPosition));
40: return keccak256(abi.encode(timeswapV2LiquidityTokenPosition));
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/Position.sol#L35
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/Position.sol#L40
35: v2-option/src/TimeswapV2OptionDeployer.sol
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2OptionDeployer.sol#L35

[L-03] Unsafe ERC20 operation(s)
128: IERC20(token0).balanceOf(address(this)) + token0AndLong0Amount,
129: IERC20(token1).balanceOf(address(this)) + token1AndLong1Amount
145: if (token0AndLong0Amount != 0) Error.checkEnough(IERC20(token0).balanceOf(address(this)), currentProcess.balance0Target);
148: if (token1AndLong1Amount != 0) Error.checkEnough(IERC20(token1).balanceOf(address(this)), currentProcess.balance1Target);
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L128
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L129
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L145
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L148
172: if (token0AndLong0Amount != 0) IERC20(token0).safeTransfer(param.token0To, token0AndLong0Amount);
175: if (token1AndLong1Amount != 0) IERC20(token1).safeTransfer(param.token1To, token1AndLong1Amount);
215: param.isLong0ToLong1 ? IERC20(token0).balanceOf(address(this)) - token0AndLong0Amount : IERC20(token0).balanceOf(address(this)) + token0AndLong0Amount,
216: param.isLong0ToLong1 ? IERC20(token1).balanceOf(address(this)) + token1AndLong1Amount : IERC20(token1).balanceOf(address(this)) - token1AndLong1Amount
235:  Error.checkEnough(IERC20(param.isLong0ToLong1 ? token1 : token0).balanceOf(address(this)), param.isLong0ToLong1 ? currentProcess.balance1Target : currentProcess.balance0Target);
259: if (token0Amount != 0) IERC20(token0).safeTransfer(param.token0To, token0Amount);
262: if (token1Amount != 0) IERC20(token1).safeTransfer(param.token1To, token1Amount);
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L172
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L175
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L215
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L216
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L235
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L259
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L262

[L-04] LOSS OF PRECISION DUE TO ROUNDING
Proof Of Concept
48: function div(uint256 dividend, uint256 divisor, bool roundUp) internal pure returns (uint256 quotient) {
49:        quotient = dividend / divisor;
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/Math.sol#L48-L49

[N-01] Constants should be defined rather than using magic numbers
Proof Of Concept
15: if ((length - 4) % 32 == 0 && bytes4(reason) == selector) return BytesLib.slice(reason, 4, length - 4);
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/CatchError.sol#L15

[N-02] Missing checks for address(0) when assigning values to address state variables
Proof Of Concept
v2-pool/src/base/OwnableTwoSteps.sol
29: pendingOwner = chosenPendingOwner;
 https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/base/OwnableTwoSteps.sol#L29

v2-token/src/TimeswapV2LiquidityToken.sol
37: optionFactory = chosenOptionFactory;
38: poolFactory = chosenPoolFactory;
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L37
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L38

[N-03] NATSPEC COMMENTS SHOULD BE INCREASED IN CONTRACTS
Context:
All Contracts

Description:
It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation.
In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.
https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

Recommendation:
NatSpec comments should be increased in contracts

[N-04] NON-LIBRARY/INTERFACE FILES SHOULD USE FIXED COMPILER VERSIONS, NOT FLOATING ONES
Proof Of Concept
2: pragma solidity ^0.8.8;
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L2
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L2
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/CallbackParam.sol#L2
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/FeesPosition.sol#L2
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/Param.sol#L2
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/Position.sol#L2

[N-05] Functions not used internally could be marked external
Proof Of Concept
v2-token/src/TimeswapV2Token.sol
66: function positionOf(address owner, TimeswapV2TokenPosition calldata timeswapV2TokenPosition) public view returns (uint256 amount) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L66