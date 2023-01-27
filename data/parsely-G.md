- To save Gas it should be considered to mark constructors as payable.

- Due to the external functions never using the ```msg.value``` amount in any way, it can be considered to mark external functions also as payable this can possibly save on op codes without negative impact, as checking for zero msg values uses gas to run the following opcodes.
ALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2) about 21 gas per call.

Below is an example of the current code state with functions not marked as payable:
```
| src/TimeswapV2Option.sol:TimeswapV2Option contract |                 |        |        |        |         |
|----------------------------------------------------|-----------------|--------|--------|--------|---------|
| Deployment Cost                                    | Deployment Size |        |        |        |         |
| 2640866                                            | 13631           |        |        |        |         |
| Function Name                                      | min             | avg    | median | max    | # calls |
| burn                                               | 19251           | 19272  | 19272  | 19294  | 2       |
| collect                                            | 63948           | 64087  | 63976  | 64337  | 3       |
| mint                                               | 250064          | 304127 | 319493 | 320248 | 9       |
| swap                                               | 155303          | 155309 | 155309 | 155315 | 2       |
```

Below is an example of the same contract with the functions marked as payable.
```
| src/TimeswapV2Option.sol:TimeswapV2Option contract |                 |        |        |        |         |
|----------------------------------------------------|-----------------|--------|--------|--------|---------|
| Deployment Cost                                    | Deployment Size |        |        |        |         |
| 2656489                                            | 13709           |        |        |        |         |
| Function Name                                      | min             | avg    | median | max    | # calls |
| burn                                               | 19227           | 19248  | 19248  | 19270  | 2       |
| collect                                            | 63924           | 64043  | 64084  | 64123  | 3       |
| mint                                               | 250052          | 304075 | 319417 | 319988 | 9       |
| swap                                               | 155284          | 155290 | 155290 | 155296 | 2       |
```