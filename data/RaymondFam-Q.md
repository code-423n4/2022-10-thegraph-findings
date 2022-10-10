## Missing Require Error Message
Consider adding a less than 32 character string message to all require statements just so that a relevant message would be displayed in case of a revert. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L47
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L59

## Unspecific Compiler Version Pragma
For most source-units the compiler version pragma is very unspecific ^0.7.6. While this often makes sense for libraries to allow them to be included with multiple different versions of an application, it may be a security risk for the actual application implementation itself. A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up actually checking a different EVM compilation that is ultimately deployed on the blockchain.

Avoid floating pragmas where possible. It is highly recommend pinning a concrete compiler version (latest without security issues) in at least the top-level “deployed” contracts to make it unambiguous which compiler version is being used. Rule of thumb: a flattened source-unit should have at least one non-floating concrete solidity compiler version pragma.

## Solidity Version
Consider using the latest released version of Solidity. Apart from exceptional cases, only the latest version receives security fixes. Furthermore, breaking changes as well as new features are introduced regularly.

## No Storage Gap for Upgradeable Contracts
Consider adding a storage gap at the end of each upgradeable contract, just in case it would entail some child contracts in the future. This would ensure no shifting down of storage in the inheritance chain. Only `Managed.sol` has been found to be doing this:

```
uint256[50] private __gap;
```

Typically, storage gaps are a convention for reserving storage slots in a base contract, allowing future versions of that contract to use up those slots without affecting the storage layout of child contracts. Otherwise, the variable in the child contract might be overwritten by the upgraded implementation code whenever new variables are added to it. This could have unintended and vulnerable consequences to the child contracts.

Please visit the bottom part of article below for further details:

https://docs.openzeppelin.com/upgrades-plugins/1.x/writing-upgradeable

## Use `immutable` Instead of `constant` Whose Value is Expressed as a Call to `keccak256()`
Despite not saving any gas, it’s part of the best practices to adopt `immutable` variables associated with expressions for calculated values, and better yet, have them passed into the constructor where possible. `constant` variables, on the other hand, are more appropriately used for literal values written into the code. Here are some of the instances entailed:

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L34-L39
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L42-L45
