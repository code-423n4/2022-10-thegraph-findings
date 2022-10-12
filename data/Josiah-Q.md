## REMOVE COMMENTED CODE
In the code base of `GraphProxyAdmin.sol`, there are a few lines of code that have been commented out with //. This can lead to confusion and not conducive to overall code readability. Consider removing/rewriting them. There are three instances found.

[Line 32](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L32)
[Line 45](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L45)
[Line 57](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L57)

## STORAGE GAP FOR UPGRADEABLE CONTRACTS
Consider adding a storage gap at the end of each upgradeable contract. In the event some contracts needed to inherit from them, there would not be an issue shifting down of storage in the inheritance chain. Generally, storage gaps are a novel way of reserving storage slots in a base contract, allowing future versions of that contract to use up those slots without affecting the storage layout of child contracts. If not, the variable in the child contract might be overridden whenever new variables are added to it. This could have unintended and vulnerable consequences to the child contracts.

## UPGRADE TO NEWER VERSION OF SOLIDITY
Consider using a newer version of Solidity ^0.8.0 that features the latest security fixes other than breaking changes and new features introduced periodically.

## REQUIRE STATEMENT WITH MISSING ERROR MESSAGE
Consider adding a RELEVANT message to all require statements that would be displayed in the event of a revert. There are two instances found in `GraphProxyAdmin.sol`.

[Line 47](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L47)
[Line 59](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L59)

Consider using the latest released version of Solidity. Apart from exceptional cases, only the latest version receives security fixes. Furthermore, breaking changes as well as new features are introduced regularly.

## PRIVILEGED CALLS AND IMPLEMENTATIONS COULD FRONT RUN USERS UNEXPECTEDLY
Administrators can update or upgrade the contracts without warning, which violates the security goal of the system. Specifically, privileged roles could use front running to make malicious changes just ahead of incoming transactions, or purely accidental negative effects could occur due to the unfortunate timing of changes. Generally, users of the system should have assurances about the behavior of the action they are about to take.

Consider giving users advance notice of changes with a time lock. The first step merely broadcasts to users that a particular change is coming, and the second step commits that change after a suitable waiting period. This would allow users that do not accept the change to withdraw immediately.

## COMPILER VERSION PRAGMA SPECIFICITY
Non-library contracts and interfaces should avoid using floating pragmas ^0.7.6. Doing this may be a security risk for the actual application implementation itself. For instance, a known vulnerable compiler version may accidentally be selected or a security tool might fallback to an older compiler version leading to checking a different EVM compilation that is ultimately deployed on the blockchain.

## IMMUTABLE INSTEAD OF CONSTANT
Variables having expressions for calculated values should be linked with `immutable` instead of `constant`. There are two instances found in. There are quite a number of instances found in `GraphTokenUpgradeable.sol`.

[Lines 34-39](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L34-L39)
[Lines 42-45](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L42-L45)

## ESCROW FUND COULD BE DRAINED
Hackers could exploit via numerous attacking surfaces using low level proofs to hijack the tree system to receive GRT from the escrow simply because there is a long delay to synchronize states on both the EVM and the AVM chains. A more rigorous checks should be in place rather than just counting on the following require statement in `L1GraphTokenGateway.sol`.

[Line 275](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L275)

It would too late when the above require statement reverted when all funds would have been drained. It belies the cross-chain atomicity alleged in Arbitrum's documentations.