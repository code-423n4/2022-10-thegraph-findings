## Variable declare but not use
- Contract `Managed` not use variable `__gap`

## Internal functions only called once can be inlined to save gas
Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

Should move all `require` into each fit modifier 
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L43-L58

## Add return variables to save gas

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L286-L296

```solidity
function parseOutboundData(bytes memory _data) private view returns (address from, bytes memory extraData) {
        if (msg.sender == l2Router) {
            (from, extraData) = abi.decode(_data, (address, bytes));
        } else {
            from = msg.sender;
            extraData = _data;
        }
}
```