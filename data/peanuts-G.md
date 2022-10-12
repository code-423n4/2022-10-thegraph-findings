## TABLE OF CONTENTS

- [G-01] Using > 0 costs more gas than != 0 when used on a uint in a require() statement
- [G-02] Updating solidity version to the latest saves gas
- [G-03] Use custom errors rather than revert()/require() strings to save deployment gas

## [G-01] Using > 0 costs more gas than != 0 when used on a uint in a require() statement

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L201
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L217
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L146

## [G-02] Updating solidity version to the latest saves gas

Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than `revert()/require()` strings Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value
https://swcregistry.io/docs/SWC-103

## [G-03] Use custom errors rather than revert()/require() strings to save deployment gas

Custom errors are available from solidity version 0.8.4.

eg. 
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L110-L111