# **Low and Non-Critical**

Serial No. | Issue Name | Instances
--- | --- | ---
Q-1 | Use safeTransfer/safeTransferFrom consistently instead of transfer/transferFrom | 2
Q-2 | USE SAFEAPPROVE INSTEAD OF APPROVE | 1
Q-3 | The nonReentrant modifier should occur before all other modifiers | 2
Q-4 | _safeMint() should be used rather than _mint() wherever possible | 3
Q-5 | Avoid using Floating Pragma | 20
Q-6 | Use of Block.Timestamp | 3
Q-7 | Multiple initialization due to initialize function not having initializer modifier | 8

-----
***Total instances found in this contest: 39 | Among all files in scope***


## 1. Use safeTransfer/safeTransferFrom consistently instead of transfer/transferFrom

It is good to add a require() statement that checks the return value of token transfers or to use something like OpenZeppelin’s safeTransfer/safeTransferFrom unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract.

### Instances:
[L1GraphTokenGateway.sol:L235](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L235)
```
contracts/gateway/L1GraphTokenGateway.sol:235:                token.transferFrom(from, escrow, _amount);
```
[L1GraphTokenGateway.sol:L276](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L276)
```
contracts/gateway/L1GraphTokenGateway.sol:276:        token.transferFrom(escrow, _to, _amount);
```
### Reference:

This [similar medium-severity finding](https://consensys.net/diligence/audits/2021/01/fei-protocol/#unchecked-return-value-for-iweth-transfer-call) from Consensys Diligence Audit of Fei Protocol.

### Recommended Mitigation Steps:

Consider using safeTransfer/safeTransferFrom or require() consistently.


-----

## 2. USE SAFEAPPROVE INSTEAD OF APPROVE

This is probably an oversight since SafeERC20 was imported and safeTransfer() was used for ERC20 token transfers. Nevertheless, note that approve() will fail for certain token implementations that do not return a boolean value (). Hence it is recommend to use safeApprove().

### Instances:
[BridgeEscrow.sol:L29](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/BridgeEscrow.sol#L29)
```
contracts/gateway/BridgeEscrow.sol:29:        graphToken().approve(_spender, type(uint256).max);
```
### Recommended Mitigation Steps

Update to safeapprove in the function.


-----
## 3. The nonReentrant modifier should occur before all other modifiers
### Description
The nonReentrant modifier should occur before all other modifiers
This is a best-practice to protect against re-entrancy in other modifiers
### Instances 
[L2GraphTokenGateway.sol:L144](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L144)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:144:    ) public payable override notPaused nonReentrant returns (bytes memory) {
```
[L2GraphTokenGateway.sol:L232](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L232)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:232:    ) external payable override notPaused onlyL1Counterpart nonReentrant {
```
### Recommended Mitigation Steps
using the nonReentrant modifier before onlyWhileActive modifier

-----
## 4. _safeMint() should be used rather than _mint() wherever possible
### Description:
`_mint()` is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of `_safeMint()` which ensures that the recipient is either an EOA or implements `IERC721Receiver`. Both open [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) and [solmate](https://github.com/Rari-Capital/solmate/blob/4eaf6b68202e36f67cab379768ac6be304c8ebde/src/tokens/ERC721.sol#L180) have versions of this function so that NFTs aren’t lost if they’re minted to contracts that cannot transfer them back out.
### Instances
[GraphTokenUpgradeable.sol:L133](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L133)
```
contracts/l2/token/GraphTokenUpgradeable.sol:133:        _mint(_to, _amount);
```
[GraphTokenUpgradeable.sol:L155](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L155)
```
contracts/l2/token/GraphTokenUpgradeable.sol:155:        _mint(_owner, _initialSupply);
```
[L2GraphToken.sol:L81](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L81)
```
contracts/l2/token/L2GraphToken.sol:81:        _mint(_account, _amount);
```
### Recommendations:
Use _safeMint() instead of _mint().

-----

## 5. Avoid using Floating Pragma:

Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

### Instances:
[ICuration.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/ICuration.sol#L3)
```
contracts/curation/ICuration.sol:3:pragma solidity ^0.7.6;
```
[IGraphCurationToken.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/IGraphCurationToken.sol#L3)
```
contracts/curation/IGraphCurationToken.sol:3:pragma solidity ^0.7.6;
```
[L2GraphTokenGateway.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L3)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:3:pragma solidity ^0.7.6;
```
[L2GraphToken.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L3)
```
contracts/l2/token/L2GraphToken.sol:3:pragma solidity ^0.7.6;
```
[GraphTokenUpgradeable.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L3)
```
contracts/l2/token/GraphTokenUpgradeable.sol:3:pragma solidity ^0.7.6;
```
[IGraphProxy.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/IGraphProxy.sol#L3)
```
contracts/upgrades/IGraphProxy.sol:3:pragma solidity ^0.7.6;
```
[GraphProxyAdmin.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L3)
```
contracts/upgrades/GraphProxyAdmin.sol:3:pragma solidity ^0.7.6;
```
[GraphProxy.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L3)
```
contracts/upgrades/GraphProxy.sol:3:pragma solidity ^0.7.6;
```
[GraphUpgradeable.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L3)
```
contracts/upgrades/GraphUpgradeable.sol:3:pragma solidity ^0.7.6;
```
[GraphProxyStorage.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyStorage.sol#L3)
```
contracts/upgrades/GraphProxyStorage.sol:3:pragma solidity ^0.7.6;
```
[L1GraphTokenGateway.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L3)
```
contracts/gateway/L1GraphTokenGateway.sol:3:pragma solidity ^0.7.6;
```
[ICallhookReceiver.sol:L9](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/ICallhookReceiver.sol#L9)
```
contracts/gateway/ICallhookReceiver.sol:9:pragma solidity ^0.7.6;
```
[BridgeEscrow.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/BridgeEscrow.sol#L3)
```
contracts/gateway/BridgeEscrow.sol:3:pragma solidity ^0.7.6;
```
[GraphTokenGateway.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/GraphTokenGateway.sol#L3)
```
contracts/gateway/GraphTokenGateway.sol:3:pragma solidity ^0.7.6;
```
[IEpochManager.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/epochs/IEpochManager.sol#L3)
```
contracts/epochs/IEpochManager.sol:3:pragma solidity ^0.7.6;
```
[IRewardsManager.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/IRewardsManager.sol#L3)
```
contracts/rewards/IRewardsManager.sol:3:pragma solidity ^0.7.6;
```
[Pausable.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L3)
```
contracts/governance/Pausable.sol:3:pragma solidity ^0.7.6;
```
[Managed.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L3)
```
contracts/governance/Managed.sol:3:pragma solidity ^0.7.6;
```
[Governed.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L3)
```
contracts/governance/Governed.sol:3:pragma solidity ^0.7.6;
```
[IGraphToken.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/token/IGraphToken.sol#L3)
```
contracts/token/IGraphToken.sol:3:pragma solidity ^0.7.6;
```
### Recomended Mitigation Step
Recommend using fixed solidity version

### References:

[https://code4rena.com/reports/2022-04-phuture#g-20-use-a-more-recent-version-of-solidity](https://code4rena.com/reports/2022-04-phuture#g-20-use-a-more-recent-version-of-solidity)


-----


## 6. Use of Block.Timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

### Instances:
[GraphTokenUpgradeable.sol:L95](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L95)
```
contracts/l2/token/GraphTokenUpgradeable.sol:95:        require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
```
[Pausable.sol:L32](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L32)
```
contracts/governance/Pausable.sol:32:            lastPausePartialTime = block.timestamp;
```
[Pausable.sol:L46](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L46)
```
contracts/governance/Pausable.sol:46:            lastPauseTime = block.timestamp;
```
### References:

https://github.com/code-423n4/2022-04-dualityfocus-findings/issues/33


-----
## 7. Multiple initialization due to initialize function not having initializer modifier
### Description

The attacker can initialize the contract, take malicious actions, and allow it to be re-initialized by the project without any error being noticed.

### instances:
[Governed.sol:L31](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L31)
```
contracts/governance/Governed.sol:31:    function _initialize(address _initGovernor) internal {
```
[Managed.sol:L87](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L87)
```
contracts/governance/Managed.sol:87:    function _initialize(address _controller) internal {
```
[IGraphCurationToken.sol:L8](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/IGraphCurationToken.sol#L8)
```
contracts/curation/IGraphCurationToken.sol:8:    function initialize(address _owner) external;
```
[L1GraphTokenGateway.sol:L99](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L99)
```
contracts/gateway/L1GraphTokenGateway.sol:99:    function initialize(address _controller) external onlyImpl {
```
[BridgeEscrow.sol:L20](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/BridgeEscrow.sol#L20)
```
contracts/gateway/BridgeEscrow.sol:20:    function initialize(address _controller) external onlyImpl {
```
[L2GraphTokenGateway.sol:L87](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L87)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:87:    function initialize(address _controller) external onlyImpl {
```
[GraphTokenUpgradeable.sol:L150](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L150)
```
contracts/l2/token/GraphTokenUpgradeable.sol:150:    function _initialize(address _owner, uint256 _initialSupply) internal {
```
[L2GraphToken.sol:L48](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L48)
```
contracts/l2/token/L2GraphToken.sol:48:    function initialize(address _owner) external onlyImpl {
```
-----