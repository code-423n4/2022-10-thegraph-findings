## Reentrancy Possibility
External function emit an event after the reentrant code. Hence, any event emitted during the external call will appear before the event emitted by the functions above. 
While having no effects on transaction execution, such order complicates the monitoring and reconstruction of contract state based on the events info.

- Contract `L1GraphTokenGateway` 
	- Func `finalizeInboundTransferEvent` emit `WithdrawalFinalized` after external call was executed.
	- Func `outboundTransfer` emit `DepositInitiated` after external calls was executed.


## Inheritance Recommend
- Contract `GraphProxy` should inherit from `IGraphProxy`
- Contract `L2GraphToken` should inherit from `IGraphCurationToken`

## Compare to a boolean constant
Compare `callhookWhitelist[msg.sender]` with `true`
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L213-L216
Should update to
```solidity
require(
    extraData.length == 0 || callhookWhitelist[msg.sender],
    "CALL_HOOK_DATA_NOT_ALLOWED"
);
```

## Can use modifier

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L59-L125

Can update modifier onlyMinter contains params input, then we can reuse in function renounceMinter

```solidity
modifier onlyMinter(address _sender) {
        require(isMinter(_sender), "Only minter can call");
        _;
    }
```

```solidity
    function renounceMinter() external onlyMinter(msg.sender){
        _removeMinter(msg.sender);
    }

    function mint(address _to, uint256 _amount) external onlyMinter(msg.sender) {
        _mint(_to, _amount);
    }
```

## Event should indexed amount
Contract `L2GraphToken`
For easier to tracking or managing amount of new token was minted or burned. 
We should add indexed to event `BridgeMinted` and `BridgeBurned`

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L24-L26

```solidity
event BridgeMinted(address indexed account, uint256 indexed amount);
event BridgeBurned(address indexed account, uint256 indexed amount);
```

## Missing error messages
Should add error message when condition in require was fail.

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L34
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L47
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L59
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L133

## Missing event emit
Should emit events after write functions to make tracking process better.
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L68-L70
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L77-L79
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L86-L88

## USE A MORE RECENT VERSION OF SOLIDITY : < 0.8
Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining 
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads 
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings 
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value


## NON-LIBRARY/INTERFACE FILES SHOULD USE FIXED COMPILER VERSIONS, NOT FLOATING ONES

Avoid floating pragmas for non-library contracts.
While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.
A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.
It is recommended to pin to a concrete compiler version.

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/BridgeEscrow.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L3
...

