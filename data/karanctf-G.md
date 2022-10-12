
## [G-1] Variable == false|0 -> !variable or variable ==  true -> variable
```solidity
L1GraphTokenGateway.sol:214:                    extraData.length == 0 || callhookWhitelist[msg.sender] == true,
GraphTokenUpgradeable.sol:95:        require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
L2GraphTokenGateway.sol:147:        require(msg.value == 0, "INVALID_NONZERO_VALUE");
L2GraphTokenGateway.sol:153:        require(outboundCalldata.extraData.length == 0, "CALL_HOOK_DATA_NOT_ALLOWED");
L2GraphTokenGateway.sol:234:        require(msg.value == 0, "INVALID_NONZERO_VALUE");
```

## [G-2] Use `!= 0` instead of `> 0`
```solidity
L1GraphTokenGateway.sol:201:        require(_amount > 0, "INVALID_ZERO_AMOUNT");
L1GraphTokenGateway.sol:217:                require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
IStaking.sol:15:     * - Active = not Null && tokens > 0
L2GraphTokenGateway.sol:146:        require(_amount > 0, "INVALID_ZERO_AMOUNT");
L2GraphTokenGateway.sol:238:        if (_data.length > 0) {
```
## [G-3] Use `abi.encodePacked` for gas optimization 
Changing the abi.encode function to abi.encodePacked  can save gas since the abi.encode function pads extra null bytes at the end of the call data, which is unnecessary. Also, in general, abi.encodePacked is more gas-efficient.
```solidity
L1GraphTokenGateway.sol:249:        return abi.encode(seqNum);
L1GraphTokenGateway.sol:342:                abi.encode(emptyBytes, _data)
GraphTokenUpgradeable.sol:88:                    abi.encode(PERMIT_TYPEHASH, _owner, _spender, _value, nonces[_owner], _deadline)
GraphTokenUpgradeable.sol:162:            abi.encode(
L2GraphTokenGateway.sol:174:        return abi.encode(id);
L2GraphTokenGateway.sol:275:                abi.encode(0, _data) // we don't need to track exitNums (b/c we have no fast exits) so we always use 0
```
