
## G01 Saving SLOAD's. 9 in total (~900 gas)

```L1GraphTokenGateway.sol```
1 - ```inbox``` is accessed twice (line 74 and line 77). cache ```inbox``` and use from stack to save 1 SLOAD.
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L74-L77
2 - ```escrow``` is accessed twice (line 273 and line 276). cache ```escrow``` and use from stack to save 1 SLOAD.
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L273-L276

```L2GraphTokenGateway.sol```
3 - ```l1GRT``` is accessed twice (line 145 and line 156). cache ```l1GRT``` and use from stack to save 1 SLOAD.
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L145-L156
4 - ```l1GRT``` is accessed twice (line 233 and line 236). cache ```l1GRT``` and use from stack to save 1 SLOAD.
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L233-L236

```Pausable.sol```
5 - replace ```_partialPaused``` with ```_toPause``` to save 1 SLOAD.
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L31
6 - replace ```_partialPaused``` with ```_toPause``` to save 1 SLOAD.
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L34
7 - replace ```_paused``` with ```_toPause``` to save 1 SLOAD.
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L45
8 - replace ```_paused``` with ```_toPause``` to save 1 SLOAD.
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L48
9 - replace ```pauseGuardian``` with ```newPauseGuardian``` to save 1 SLOAD.
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L58





## G02 Change 3 bool state variables to uint8 (each ~20K gas, ~60K total)
```    
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.
```
https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27

Consider changing the bool's ```_partialPaused```, ```_paused``` and ```callhookWhitelist``` to uint and use values ```1``` and ```2``` rather than ```true``` and ```false```.
This would save an extra SLOAD(100), and an additional Gsset(20K) when changing from ```false` to ```true```.

### Lines of code
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L8
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L10
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L35




## G03 Error strings longer than 32 characters (each 6 gas, 36 total)

Error strings take space in the deployed bytecode. Every reason string takes at least 32 bytes so make sure your string fits in 32 bytes or it will become more expensive.

### Lines of code
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L105
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L141
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L144
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol#L21
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#L32
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L53
