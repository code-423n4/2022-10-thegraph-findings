   # 1 Use != 0 instead of > 0 at the above mentioned codes. The variable is uint, so it will not be below 0 so it can just check != 0.
!= 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L238
# 2  REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS
Each extra memory word of bytes past the original 32 incurs an MSTORE which costs 3 gas
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L141


# 3  USE MULTIPLE REQUIRE() STATMENTS INSTED OF REQUIRE(EXPRESSION && EXPRESSION && ...)
SPLITTING REQUIRE() STATEMENTS THAT USE && SAVES GAS
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L142
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L142