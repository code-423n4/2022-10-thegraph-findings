- Use a more recent version of Solidity.
	- Use a solidity version of at least 0.8.0 to get overflow protection without `SafeMath`; Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining; Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads; Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than `revert()/require()` strings; Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value.


- Splitting `REQUIRE()` statements that use && saves gas.
	- Instances:
		- https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L55
		- https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L142



- Using `BOOLS` for storage incurs overhead.
	- Use `uint256(1)` and `uint256(2)` for `true/false` to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past.
	- Instances:
		- https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L8-L10


- `ABI.ENCODE()` is less efficient than `ABI.ENCODEPACKED()`.
	- Instances:
		- https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L174
		- https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L249

