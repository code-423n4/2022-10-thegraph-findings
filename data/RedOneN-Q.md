## [N-1] Use a more recent version of solidity.

Use a solidity version of at least 0.8.0 to get overflow protection without SafeMath, Use a solidity version of at least 0.8.2 to get compiler automatic inlining,Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads,Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings,Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value


Governed.sol#L3
BridgeEscrow.sol#L3
GraphProxyAdmin.sol#L3
GraphProxy.sol#L3
Pausable.sol#L3
L2GraphToken.sol#L3
GraphTokenUpgradeable.sol#L3
GraphUpgradeable.sol#L3
GraphProxyStorage.sol#L3
GraphTokenGateway.sol#L3
L1GraphTokenGateway.sol#L3
L2GraphTokenGateway.sol#L3
Managed.sol#L3



## [N-2] Expressions for constant values such as a call to KECCAK256() should use immutable rather than constant.

While it doesn’t save any gas because the compiler knows that developers often make this mistake, it’s still best to use the right tool for the task at hand. There is a difference between constant variables and immutable variables, and they should each be used in their appropriate contexts. constants should be used for literal values written into the code, and immutable variables should be used for expressions, or values calculated in, or passed into the constructor.

***File : GraphTokenUpgradeable.sol***

GraphTokenUpgradeable.sol#L38
GraphTokenUpgradeable.sol#L39


## [N-3] Use of block.timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

***File : Pausable.sol***

Pausable.sol#L32
Pausable.sol#L46

***File : GraphTokenUpgradeable.sol***

GraphTokenUpgradeable.sol#L95




## [N-4] Require() should be used instead of assert().

The assert function creates an error of type Panic(uint256). … Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix.

***File : GraphProxy.sol***

GraphProxy.sol#L47
GraphProxy.sol#L48
GraphProxy.sol#L51
