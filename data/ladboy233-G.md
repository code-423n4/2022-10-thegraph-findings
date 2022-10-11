## function can be marked as external instead of public


https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L30


```solidity
    function getProxyImplementation(IGraphProxy _proxy) public view returns (address) {
```
            

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L43


```solidity
    function getProxyPendingImplementation(IGraphProxy _proxy) public view returns (address) {
```
            

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L55


```solidity
    function getProxyAdmin(IGraphProxy _proxy) public view returns (address) {
```
            

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L68


```solidity
    function changeProxyAdmin(IGraphProxy _proxy, address _newAdmin) public onlyGovernor {
```
            

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L77


```solidity
    function upgrade(IGraphProxy _proxy, address _implementation) public onlyGovernor {
```
            

## ABI.ENCODE() IS LESS EFFICIENT THAN ABI.ENCODEPACKED()
   

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L88


```solidity
                    abi.encode(PERMIT_TYPEHASH, _owner, _spender, _value, nonces[_owner], _deadline)
```
            

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L162


```solidity
            abi.encode(
```
            

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L174


```solidity
        return abi.encode(id);
```
            

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L269


```solidity
            abi.encodeWithSelector(
```
            

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L275


```solidity
                abi.encode(0, _data) // we don't need to track exitNums (b/c we have no fast exits) so we always use 0
```
            

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L249


```solidity
        return abi.encode(seqNum);
```
            

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L336


```solidity
            abi.encodeWithSelector(
```
            

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L342


```solidity
                abi.encode(emptyBytes, _data)
```
            

## Splitting require() statements that use && saves gas

https://github.com/code-423n4/2022-01-xdefi-findings/issues/128

See this issue which describes the fact 
that there is a larger deployment gas cost, 
but with enough runtime calls, the change ends up being cheaper


https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L142


```solidity
        require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
```