🐞 Presence of unused variables (Informational)

Unused variables are allowed in Solidity and they do not pose a direct security issue. It is best practice though to avoid them as they can:

cause an increase in computations (and unnecessary gas consumption)
indicate bugs or malformed data structures and they are generally a sign of poor code quality
cause code noise and decrease readability of the code

Remediation
Remove all unused variables from the code base.

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L29



🐞 Pragma version (Informational)
solc frequently releases new compiler versions. Using an old version prevents access to new Solidity security checks. 

Recommendation
Deploy with any of the following Solidity versions

Risks related to recent releases
Risks of complex code generation changes
Risks of new language features
Risks of known bugs
Use a simple pragma version that allows any of these versions. Consider using the latest version of Solidity for testing.

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/IController.sol#L3

contracts/staking/IStakingData.sol#L3
contracts/staking/IStaking.sol# L3




