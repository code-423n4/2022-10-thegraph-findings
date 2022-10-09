## Use safeTransferFrom instead of transferFrom

Contract:
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L235

Issue:
It was observed that outboundTransfer was using transferFrom function instead of safeTransferFrom. This means even if transfer is not success, contract will have no way to know the same and would process the transaction without receiving the funds

Recommendation:
Kindly use safeTransferFrom instead of transferFrom