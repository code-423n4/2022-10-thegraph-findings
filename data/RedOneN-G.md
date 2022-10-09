## [G-1] Use custom error rather than require() with string explanation.

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they’re hit by avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas.

***File : Governed.sol***

Governed.sol#L24
Governed.sol#L41

***File : GraphProxy.sol***

GraphProxy.sol#L105
GraphProxy.sol#L141
GraphProxy.sol#L157

***File : L2GraphToken.sol***

L2GraphToken.sol#L36
L2GraphToken.sol#L49
L2GraphToken.sol#L60
L2GraphToken.sol#L70

***File : GraphTokenUpgradeable.sol***

GraphTokenUpgradeable.sol#L60
GraphTokenUpgradeable.sol#L94
GraphTokenUpgradeable.sol#L95
GraphTokenUpgradeable.sol#L106
GraphTokenUpgradeable.sol#L115
GraphTokenUpgradeable.sol#L123

***File : GraphUpgradeable.sol***

GraphUpgradeable.sol#L24
GraphUpgradeable.sol#L32

***File : GraphProxyStorage.sol***

GraphProxyStorage.sol#L62

***File : GraphTokenGateway.sol***

GraphTokenGateway.sol#L31
GraphTokenGateway.sol#L40

***File : L1GraphTokenGateway.sol***

L1GraphTokenGateway.sol#L74
L1GraphTokenGateway.sol#L78
L1GraphTokenGateway.sol#L82
L1GraphTokenGateway.sol#L110
L1GraphTokenGateway.sol#L111
L1GraphTokenGateway.sol#L122
L1GraphTokenGateway.sol#L132
L1GraphTokenGateway.sol#L142
L1GraphTokenGateway.sol#L153
L1GraphTokenGateway.sol#L154
L1GraphTokenGateway.sol#L165
L1GraphTokenGateway.sol#L166
L1GraphTokenGateway.sol#L200
L1GraphTokenGateway.sol#L201
L1GraphTokenGateway.sol#L202
L1GraphTokenGateway.sol#L217
L1GraphTokenGateway.sol#L224
L1GraphTokenGateway.sol#L271
L1GraphTokenGateway.sol#L275

***File : L2GraphTokenGateway.sol***

L2GraphTokenGateway.sol#L98
L2GraphTokenGateway.sol#L108
L2GraphTokenGateway.sol#L118
L2GraphTokenGateway.sol#L145
L2GraphTokenGateway.sol#L146
L2GraphTokenGateway.sol#L147
L2GraphTokenGateway.sol#L148
L2GraphTokenGateway.sol#L153
L2GraphTokenGateway.sol#L233
L2GraphTokenGateway.sol#L234

***File : Managed.sol***

Managed.sol#L44
Managed.sol#L45
Managed.sol#L49
Managed.sol#L53
Managed.sol#L57
Managed.sol#L104




## [G-2] Strings longer than 32bytes inside require()/revert cost extra gas.

Each extra memory word of bytes past the original 32 incurs an MSTORE which costs 3 gas.

***File : GraphProxy.sol***

GraphProxy.sol#L105

***File : Managed.sol***

Managed.sol#L53




## [G-3] Expressions for constant values such as a call to KECCAK256() should use immutable rather than constant.

While it doesn’t save any gas because the compiler knows that developers often make this mistake, it’s still best to use the right tool for the task at hand. There is a difference between constant variables and immutable variables, and they should each be used in their appropriate contexts. constants should be used for literal values written into the code, and immutable variables should be used for expressions, or values calculated in, or passed into the constructor.

***File : GraphTokenUpgradeable.sol***

GraphTokenUpgradeable.sol#L38
GraphTokenUpgradeable.sol#L39




## [G-4] Using >0 costs more gas than !=0 when used on a UINT in a require statement.

This change saves 6 gas per instance. The optimization works until solidity version 0.8.13 where there is a regression in gas costs.

***File : L1GraphTokenGateway.sol***

L1GraphTokenGateway.sol#L201
L1GraphTokenGateway.sol#L217

***File : L2GraphTokenGateway.sol***

L2GraphTokenGateway.sol#L146




## [G-5] Multiple require statement instead of && operators saves gas.

splitting-up && operators into multiple require induces a higher deployment cost. However, with enough runtime calls, this ends up being 3gas cheaper.

***File : L1GraphTokenGateway.sol***

L1GraphTokenGateway.sol#L142




## [G-6] Require() should be used instead of assert() to save gas left during a transaction.

using require instead of assert would allow to refund the remaining gas in case of error.

***File : GraphProxy.sol***

GraphProxy.sol#L47
GraphProxy.sol#L48
GraphProxy.sol#L51




## [G-7] Non-strict inequalities are cheaper than strict ones.

Strict inequalities (>) are more expensive than non-strict ones (>=). This is due to some supplementary checks (ISZERO, 3 gas). This also holds true between <= and <.

***File : GraphTokenUpgradeable.sol***

GraphTokenUpgradeable.sol#L51
GraphTokenUpgradeable.sol#L52
GraphTokenUpgradeable.sol#L95

***File : L1GraphTokenGateway.sol***

L1GraphTokenGateway.sol#L35
L1GraphTokenGateway.sol#L201
L1GraphTokenGateway.sol#L217
L1GraphTokenGateway.sol#L224
L1GraphTokenGateway.sol#L275

***File : L2GraphTokenGateway.sol***

L2GraphTokenGateway.sol#L146
L2GraphTokenGateway.sol#L238

***File : Managed.sol***

Managed.sol#L28




## [G-8] Functions guaranteed to revert when called by normal users (eg. onlyOwner) can be marked payable.

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

***File : Governed.sol***

Governed.sol#L40

***File : BridgeEscrow.sol***

BridgeEscrow.sol#L28
BridgeEscrow.sol#L36
BridgeEscrow.sol#L20

***File : GraphProxyAdmin.sol***

GraphProxyAdmin.sol#L77
GraphProxyAdmin.sol#L86
GraphProxyAdmin.sol#L68

***File : L2GraphToken.sol***

L2GraphToken.sol#L80
L2GraphToken.sol#L59
L2GraphToken.sol#L48
L2GraphToken.sol#L69
L2GraphToken.sol#L90

***File : GraphTokenUpgradeable.sol***

GraphTokenUpgradeable.sol#L105
GraphTokenUpgradeable.sol#L132
GraphTokenUpgradeable.sol#L114

***File : GraphUpgradeable.sol***

GraphUpgradeable.sol#L50

***File : GraphTokenGateway.sol***

GraphTokenGateway.sol#L47
GraphTokenGateway.sol#L30

***File : L1GraphTokenGateway.sol***

L1GraphTokenGateway.sol#L109
L1GraphTokenGateway.sol#L121
L1GraphTokenGateway.sol#L152
L1GraphTokenGateway.sol#L141
L1GraphTokenGateway.sol#L131
L1GraphTokenGateway.sol#L164
L1GraphTokenGateway.sol#L99

***File : L2GraphTokenGateway.sol***

L2GraphTokenGateway.sol#L97
L2GraphTokenGateway.sol#L117
L2GraphTokenGateway.sol#L107
L2GraphTokenGateway.sol#L87

***File : Managed.sol***

Managed.sol#L95




## [G-9] Duplicated require()/revert() checks should be refactored to a modifier to save gas.

When a require statement is used multiple times, it is cheaper in deployment costs to use a modifier instead.

***File : GraphProxyAdmin.sol***

require(success);
GraphProxyAdmin.sol#L34
GraphProxyAdmin.sol#L47
GraphProxyAdmin.sol#L59

***File : L1GraphTokenGateway.sol***

require(_l1token==address(token),"token_not_grt");
L1GraphTokenGateway.sol#L200
L1GraphTokenGateway.sol#L271

***File : L2GraphTokenGateway.sol***

require(_l1token==l1grt,"token_not_grt");
L2GraphTokenGateway.sol#L145
L2GraphTokenGateway.sol#L233
require(msg.value==0,"invalid_nonzero_value");
L2GraphTokenGateway.sol#L147
L2GraphTokenGateway.sol#L234

***File : Managed.sol***

require(!controller.paused(),"paused");
Managed.sol#L44
Managed.sol#L49




## [G-10] Don't compare BOOLEAN expressions to BOOLEAN literals?

if(<x> == true) should be restated as if(<x>). Similarly, if(<x> == false) should be restated as if(!<x>).

***File : L1GraphTokenGateway.sol***

L1GraphTokenGateway.sol#L214




## [G-11] multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.

***File : GraphTokenUpgradeable.sol***

GraphTokenUpgradeable.sol#L51
GraphTokenUpgradeable.sol#L52




## [G-12] Use a more recent version of solidity.

Use a solidity version of at least 0.8.0 to get overflow protection without SafeMath, Use a solidity version of at least 0.8.2 to get compiler automatic inlining,Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads,Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings,Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

