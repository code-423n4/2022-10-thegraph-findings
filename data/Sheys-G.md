# Lines of code

1. <https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L201>
2. <https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L217>
3. <https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L146>
4. <https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L238>

# Gas Usage Warning
The use of the `>` operand in these lines of codes in the L1 & L2 GraphTokenGateway contract costs more gas.

    Line201:         require(_amount > 0, "INVALID_ZERO_AMOUNT");
    
     ........
    
    Line217:         require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");

**In the L2GraphTokenGateway**

    Line146:         require(_amount > 0, "INVALID_ZERO_AMOUNT");
     
      .......
     
    Line238:        if (_data.length > 0) {

 A `!=` operand in the require statements would be more gas efficient. Here's an example:

    Line201:         require(_amount != 0, "INVALID_ZERO_AMOUNT");
     
     ........
     
    Line217:         require(maxSubmissionCost != 0, "NO_SUBMISSION_COST");

**In the L2GraphTokenGateway**

    Line146:         require(_amount != 0, "INVALID_ZERO_AMOUNT");
     
      .......
     
    Line238:        if (_data.length != 0) {

##Impact

The change in operands will help reduce gas fees 