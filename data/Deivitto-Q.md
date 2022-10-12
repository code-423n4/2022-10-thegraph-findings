
# QA
# Low

## `L1GraphTokenGateway.sol` ERC20 Tokens with fee on transfer are not supported
### Vulnerability details
There are ERC20 tokens that charge fee for every `transfer()` / `transferFrom()`.

`L1GraphTokenGateway.sol#finalizeInboundTransfer()` and `L1GraphTokenGateway.sol#outboundTransfer()` assume that the received amount is the same as the transfer amount, and uses it emit and event, while the actual transferred amount can be lower for those tokens. It is not so harmful as only affects event emitted and not real balances saved.


### Github Permalinks
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/L1GraphTokenGateway.sol#L235-L247
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/L1GraphTokenGateway.sol#L276-L278

### PoC
- Example:
```
                token.transferFrom(from, escrow, _amount);
                seqNum = sendTxToL2(
                    inbox,
                    l2Counterpart,
                    from,
                    msg.value,
                    0,
                    gasParams,
                    outboundCalldata
                );
            }
        }
        emit DepositInitiated(_l1Token, from, _to, seqNum, _amount);//@audit amount displayed can not be what expected
```

### Recommendation
Consider comparing before and after balance to get the actual transferred amount.



## _safeMint() should be used rather than _mint() wherever possible
### Impact 
In `NFTCollections.sol` and `NFTDropCollection`, eventually it is called ERC721 `_mint()`. Calling `_mint()` this way does not ensure that the receiver of the NFT is able to accept them, making possible to lose them. 

`_safeMint()` should be used with as it checks to see if a user can properly accept an NFT and reverts otherwise.

There is no check of the address provided by the mint NFT when creating the project that it implements ERC721Receiver. 

#### Details 
`_mint()` is discouraged in favor of `_safeMint()` which ensures that the recipient is either an EOA or implements IERC721Receiver. 

Both open OpenZeppelin and solmate have versions of this function so that NFTs aren’t lost if they’re minted to contracts that cannot transfer them back out.

#### References
https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271

### Github Permalinks
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/discovery/SubgraphNFT.sol#L99
### Mitigation
Use `_safeMint()` as suggested by OpenZeppelin or include the check before minting.





## block.timestamp used as time proxy 
### Summary
Risk of using `block.timestamp` for time should be considered. 
### Details
`block.timestamp` is not an ideal proxy for time because of issues with synchronization, miner manipulation and changing block times. 

This kind of issue may affect the code allowing or reverting the code before the expected deadline, modifying the normal functioning or reverting sometimes.
### References
SWC ID: 116

### Github Permalinks 
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/token/GraphTokenUpgradeable.sol#L95
        require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");



### Mitigation
- Consider the risk of using `block.timestamp` as time proxy and evaluate if block numbers can be used as an approximation for the application logic. Both have risks that need to be factored in. 
- Consider using an oracle for precision

## Use of `asserts()`
### Impact

From solidity docs: Properly functioning code should never reach a failing assert statement; if this happens there is a bug in your contract which you should fix.
With assert the user pays the gas and with require it doesn't. The ETH network gas isn't cheap and users can see it as a scam.
You have reachable asserts in the following locations (which should be replaced by `require` / are mistakenly left from development phase):

### Details
The Solidity `assert()` function is meant to assert invariants. Properly functioning code should never reach a failing assert statement. A reachable assertion can mean one of two things:

A bug exists in the contract that allows it to enter an invalid state;
The assert statement is used incorrectly, e.g. to validate inputs.

### References
SWC ID: 110

### Github Permalinks
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxy.sol#L47
        assert(ADMIN_SLOT == bytes32(uint256(keccak256("eip1967.proxy.admin")) - 1));

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxy.sol#L48
        assert(

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxy.sol#L51
        assert(


### Recommended Mitigation Steps
Substitute `asserts` with `require`/`revert`.

# Informational
## Comparison with a a boolean
### Summary
There are a number of instances where a boolean variable/function is checked.
### Details
- This check can be further simplified from `variable == true` to `variable`.

### Github Permalink
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/L1GraphTokenGateway.sol#L214
                    extraData.length == 0 || callhookWhitelist[msg.sender] == true,

### Mitigation
Simplify boolean comparisons in order to improve readability and save gas


## Missing indexed event parameters
### Summary
Events without indexed event parameters make it harder and
inefficient for off-chain tools to analyze them.
### Details
Indexed parameters (“topics”) are searchable event parameters.
They are stored separately from unindexed event parameters in an efficient manner to allow for faster access. This is useful for efficient off-chain-analysis, but it is also more costly gas-wise.
 
### Github Permalinks
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/token/L2GraphToken.sol#L28-L30
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/L1GraphTokenGateway.sol#L56-L66
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/governance/Managed.sol#L33-L34
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/governance/Pausable.sol#L19-L20
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/gateway/L2GraphTokenGateway.sol#L57-L62

### Mitigation
Consider which event parameters could be particularly useful to off-chain tools and should be indexed.

## Different versions of pragma
### Summary
Some of the contracts include an unlocked pragma, e.g., pragma solidity >=0.7.6. 
Locking the pragma helps ensure that contracts are not accidentally deployed using an old compiler version with unfixed bugs.

### Github Permalinks
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/BridgeEscrow.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphUpgradeable.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/governance/Governed.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/governance/Pausable.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/token/L2GraphToken.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxyAdmin.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxyStorage.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxy.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/governance/Managed.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/token/GraphTokenUpgradeable.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/gateway/L2GraphTokenGateway.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/L1GraphTokenGateway.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/GraphTokenGateway.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/curation/IGraphCurationToken.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/ICallhookReceiver.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/IGraphProxy.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/epochs/IEpochManager.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/governance/IController.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/token/IGraphToken.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/rewards/IRewardsManager.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/staking/IStakingData.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/curation/ICuration.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/staking/IStaking.sol

### Mitigation
Lock pragmas to a specific Solidity version. 
Consider converting >= 0.6.12 <0.8 into 0.7.6
Consider converting ^ 0.7.6 into 0.7.6


## Use of a more recent of solidity
### Summary
since version 0.8.4, custom errors and not needing SafeMath is enabled
### Details
Use a solidity version of at least 0.8.12 to get custom errors and default SafeMath
### Github Permalinks
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/BridgeEscrow.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphUpgradeable.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/governance/Governed.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/governance/Pausable.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/token/L2GraphToken.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxyAdmin.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxyStorage.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxy.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/governance/Managed.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/token/GraphTokenUpgradeable.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/gateway/L2GraphTokenGateway.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/L1GraphTokenGateway.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/GraphTokenGateway.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/curation/IGraphCurationToken.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/ICallhookReceiver.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/IGraphProxy.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/epochs/IEpochManager.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/governance/IController.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/token/IGraphToken.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/rewards/IRewardsManager.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/staking/IStakingData.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/curation/ICuration.sol
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/staking/IStaking.sol


### Mitigation
Consider changing to pragma 0.8.4

## Maximum line length exceeded
### Summary
Long lines should be wrapped to conform with Solidity Style guidelines. 
### Details 
Lines that exceed the 79 (or 99) character length suggested by the Solidity Style guidelines. Reference: https://docs.soliditylang.org/en/v0.8.10/style-guide.html#maximum-line-length
### Github Permalinks 

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/BridgeEscrow.sol#L33

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/token/L2GraphToken.sol#L76

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/token/L2GraphToken.sol#L86

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxyAdmin.sol#L86

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxy.sol#L14

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxy.sol#L65

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxy.sol#L78

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxy.sol#L91

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/token/GraphTokenUpgradeable.sol#L16

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/token/GraphTokenUpgradeable.sol#L36

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/token/GraphTokenUpgradeable.sol#L41

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/token/GraphTokenUpgradeable.sol#L69

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/token/GraphTokenUpgradeable.sol#L88

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/gateway/L2GraphTokenGateway.sol#L20

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/gateway/L2GraphTokenGateway.sol#L23

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/gateway/L2GraphTokenGateway.sol#L33

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/gateway/L2GraphTokenGateway.sol#L47

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/gateway/L2GraphTokenGateway.sol#L211

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/gateway/L2GraphTokenGateway.sol#L215

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/gateway/L2GraphTokenGateway.sol#L216

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/gateway/L2GraphTokenGateway.sol#L217

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/gateway/L2GraphTokenGateway.sol#L218

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/gateway/L2GraphTokenGateway.sol#L275

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/L1GraphTokenGateway.sol#L18

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/L1GraphTokenGateway.sol#L176

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/L1GraphTokenGateway.sol#L178

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/L1GraphTokenGateway.sol#L179

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/L1GraphTokenGateway.sol#L180

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/L1GraphTokenGateway.sol#L181

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/L1GraphTokenGateway.sol#L183

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/L1GraphTokenGateway.sol#L220

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/L1GraphTokenGateway.sol#L258

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/L1GraphTokenGateway.sol#L323
### Mitigation
Reduce line length to less than 99 at least to improve maintainability and readability of the code 


## Missing error messages in require statements
### Summary
Require/revert statements should include error messages in order to help at monitoring the system.
### Github Permalinks
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxyAdmin.sol#L34
        require(success);

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxyAdmin.sol#L47
        require(success);

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxyAdmin.sol#L59
        require(success);
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxy.sol#L133
        require(success);
### Mitigation
Add error messages 
