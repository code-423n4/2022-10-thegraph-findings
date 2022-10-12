## Reentrancy
External function emit an event after the reentrant code. Hence, any event emitted during the external call will appear before the event emitted by the functions above. 
While having no effects on transaction execution, such order complicates the monitoring and reconstruction of contract state based on the events info.

- Contract `L1GraphTokenGateway` 
	- Func `finalizeInboundTransferEvent` emit `WithdrawalFinalized` after external call was executed.
	- Func `outboundTransfer` emit `DepositInitiated` after external calls was executed.


## Inheritance
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


##NON-LIBRARY/INTERFACE FILES SHOULD USE FIXED COMPILER VERSIONS, NOT FLOATING ONES

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/BridgeEscrow.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L3
...

