On https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L142

Instead of checking Address.isContract(_escrow) to make sure  _escrow is a contract, it would be best to pass the code hash instead and check the code hash at those addresses. So for example:

Before:
require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");

After:

require(_escrow.codehash == codehash, "INVALID_ESCROW");

This is a stronger requirement since it would guarantee that the addresses are contracts and also they have the required code hash. For the functions to pass the require statements you would need to make 2 mistakes, one for the address and the other for the code hash. The probability of making this mistake should be theoretically lower than just passing a wrong address.