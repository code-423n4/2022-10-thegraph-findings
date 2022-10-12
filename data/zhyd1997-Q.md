no need to compare `callhookWhitelist[msg.sender]` with `true`, just use `callhookWhitelist[msg.sender]` is enough.

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L214