CallbackParam.sol
PROBLEM
Struct packing
Solidity contracts have 32 bytes slots (256 bits) that are used in storage sequentially. 
By carefully ordering one's data fields in a struct, it is possible to minimize the number of slots used within a contracts storage.
This will reduce gas costs.
A bool type variable has a size of 1 byte.
A byte variable refers to 8 bit signed integers. 
The default value for a byte is 0x00 and it gets initialized with this value.
The byte type is an alias to bytes1. The bytes1 data type represents 1 byte.
Coding the bool type and the byte type closer together requires only a single slot.
PROOF OF CONCEPT
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/CallbackParam.sol#L152
152 bool isLong0ToLong1;
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/CallbackParam.sol#L155
155 bytes data;
MITIGATION
Change the arrangement of the data fields within the struct:

struct TimeswapV2PoolRebalanceCallbackParam {
    uint256 strike;
    uint256 maturity;
    uint256 long0Amount;
    uint256 long1Amount;
    bool isLong0ToLong1;
    bytes data;
}


Param.sol
PROBLEM
Struct packing
Solidity contracts have 32 bytes slots (256 bits) that are used in storage sequentially. 
By carefully ordering one's data fields in a struct, it is possible to minimize the number of slots used within a contracts storage.
This will reduce gas costs.
An address type field has a 20 bytes size.
A bool type variable has a size of 1 byte.
An enum member is a uint8 integer type. This has a size of 1 byte. i.e TimeswapV2PoolRebalance transaction has a size of 1 byte.
The byte type is an alias to bytes1. The bytes1 data type represents 1 byte.
By coding these data fields sequentially, one would require only 1 slot
PROOF OF CONCEPT
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol#L42
42 uint256 delta;
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol#L64
64 uint256 delta;
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol#L83
83 uint256 delta;
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol#L104
104 uint256 delta;
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol#L124
124 bool isLong0ToLong1;
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol#L127
127 bytes data;
MITIGATION
Change the arrangement of the data fields within the struct:

struct TimeswapV2PoolRebalanceParam {
    uint256 strike;
    uint256 maturity;
    uint256 delta;
    address to;
    bool isLong0ToLong1;
    TimeswapV2PoolRebalance transaction;
    bytes data;
}

and another example:

struct TimeswapV2PoolMintParam {
    uint256 strike;
    uint256 maturity;
    uint256 delta;
    address to;
    TimeswapV2PoolMint transaction;
    bytes data;
}



TimeswapV2Token.sol
PROBLEM
Use abi.encodePacked() not abi.encode()
`abi.encode` pads extra null bytes at the end of the call data which is normally unnecessary. In general, `abi.encodePacked` is more gas-efficient.
PROOF OF CONCEPT
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L46
46  bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L53
53  bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L61
61  bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
MITIGATION
You should change abi.encode to abi.encodePacked

TimeswapV2LiquidityToken.sol
PROBLEM
Use assembly to check for address(0)
This will save 6 gas per instance
PROOF OF CONCEPT
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L76
76  if (from == address(0)) Error.zeroAddress();
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L77
77  if (to == address(0)) Error.zeroAddress();
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L249
249 if (from != address(0)) _feesPositions[id][from].update(uint160(balanceOf(from, id)), long0FeeGrowth, long1FeeGrowth, shortFeeGrowth);
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L251
251 if (to != address(0)) _feesPositions[id][to].update(uint160(balanceOf(to, id)), long0FeeGrowth, long1FeeGrowth, shortFeeGrowth);
MITIGATION
You can use assembly to check for address(0)

ERC1155Enumerable.sol
PROBLEM
Use assembly to check for address(0)
This will save 6 gas per instance
PROOF OF CONCEPT
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L59
59 if (from == address(0)) {/* .... */}
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L64
64 if (to != address(0) && to != from) {/* .... */}
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L93
93 if (to == address(0)) {/* .... */}
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L98
98 if (from != address(0) && from != to) {/* .... */}
MITIGATION
You can use assembly to check for address(0)

Param.sol
PROBLEM
Struct packing
Solidity contracts have 32 bytes slots (256 bits) that are used in storage sequentially. 
By carefully ordering one's data fields in a struct, it is possible to minimize the number of slots used within a contracts storage.
This will reduce gas costs.
A structs in the links provided can have their data member rearranged to save deployemnt costs
PROOF OF CONCEPT
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L20
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L26
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L44
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L67
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L72
72 bool isLong0ToLong1;
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L75
75 bytes data;
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L89
MITIGATION
Change the arrangement of the data fields within the struct: e.g.

struct TimeswapV2OptionCollectParam {
    uint256 strike;
    uint256 maturity;
    uint256 amount;
    address token0To;
    address token1To;
    TimeswapV2OptionCollect transaction;
    bytes data;
}
