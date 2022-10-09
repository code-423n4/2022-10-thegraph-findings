-> EVENT IS MISSING INDEXED FIELDS
Each event should use three indexed fields if there are three or more fields.

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#:~:text=event%20DepositFinalized(,)%3B
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#:~:text=event%20WithdrawalInitiated(,)%3B
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#:~:text=event%20DepositInitiated(,)%3B
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#:~:text=event%20WithdrawalFinalized(,)%3B


-> _SAFEMINT() SHOULD BE USED RATHER THAN _MINT() WHEREVER POSSIBLE

_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#:~:text=external%20override%20onlyGateway%20%7B-,_mint,-(_account%2C%20_amount)%3B
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#:~:text=)%20external%20onlyMinter%20%7B-,_mint,-(_to%2C%20_amount)%3B
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#:~:text=supply%20of%20tokens-,_mint,-(_owner%2C%20_initialSupply)%3B


->USE A MORE RECENT VERSION OF SOLIDITY

Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol#:~:text=pragma%20solidity%20%5E0.7.6%3B
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#:~:text=pragma%20solidity%20%5E0.7.6%3B
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#:~:text=pragma%20solidity%20%5E0.7.6%3B
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStaking.sol#:~:text=pragma%20solidity%20%3E%3D0.6.12%20%3C0.8.0%3B
