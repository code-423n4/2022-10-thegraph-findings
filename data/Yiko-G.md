https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L201
Because uints are always equal to or bigger than 0 using "!= 0" instead of "> 0" might save gas.

