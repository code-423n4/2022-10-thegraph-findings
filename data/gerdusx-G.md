 ## Gas Optimizations
 ### [G-1] Short require/revert strings save gas
 Each extra memory word of bytes past the original 32 incurs an MSTORE which costs 3 gas 

 There are 4 occurrences:
```
 Managed.sol:53	         require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
 GraphProxy.sol:105	         require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
 GraphProxy.sol:141	         require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
 GraphProxy.sol:142	         require(_pendingImplementation != address(0) && msg.sender == _pendingImplementation,"Caller must be the pending implementation");
```
 ### [G-2] Use != 0 instead of > 0
 Up until Solidity 0.8.13: != 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas) 

 There are 2 occurrences:
```
 L1GraphTokenGateway.sol:201	         require(_amount > 0, "INVALID_ZERO_AMOUNT");
 L2GraphTokenGateway.sol:146	         require(_amount > 0, "INVALID_ZERO_AMOUNT");
```
 ### [G-3] Using storage instead of memory for structs/arrays
 When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read. Instead of declearing the variable with the memory keyword, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a memory variable, is if the full struct/array is being returned by the function, is being passed to a function that requires memory, or if the array/struct is being read from another memory array/struct 

 There is 1 occurrence:
```
 L2GraphTokenGateway.sol:150	         OutboundCalldata memory outboundCalldata;
```
 ### [G-4] State variables should be cached in stack variables rather than re-reading them from storage
 The instances below point to the second+ access of a state variable within a function. Caching of a state variable replace each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses. 

 There are 4 occurrences:
```
 L1GraphTokenGateway.sol:276	 (escrow) =>         token.transferFrom(escrow, _to, _amount);
 Governed.sol:55	 (pendingGovernor) =>             pendingGovernor != address(0) && msg.sender == pendingGovernor,
 Governed.sol:60	 (pendingGovernor) =>         address oldPendingGovernor = pendingGovernor;
 Governed.sol:62	 (pendingGovernor) =>         governor = pendingGovernor;
```
 ### [G-5] Using bools for storage incurs overhead
 ```
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.
```

Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from 'false' to 'true', after having been 'true' in the past 

 There are 4 occurrences:
```
 L1GraphTokenGateway.sol:35	     mapping(address => bool) public callhookWhitelist;
 Pausable.sol:8	     bool internal _partialPaused;
 Pausable.sol:10	     bool internal _paused;
 GraphTokenUpgradeable.sol:51	     mapping(address => bool) private _minters;
```
 ### [G-6] Use a more recent version of solidity
 Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value 

 There are 12 occurrences:
```
 BridgeEscrow.sol:3	 pragma solidity ^0.7.6;
 L1GraphTokenGateway.sol:3	 pragma solidity ^0.7.6;
 Governed.sol:3	 pragma solidity ^0.7.6;
 Managed.sol:3	 pragma solidity ^0.7.6;
 Pausable.sol:3	 pragma solidity ^0.7.6;
 L2GraphTokenGateway.sol:3	 pragma solidity ^0.7.6;
 GraphTokenUpgradeable.sol:3	 pragma solidity ^0.7.6;
 L2GraphToken.sol:3	 pragma solidity ^0.7.6;
 GraphProxy.sol:3	 pragma solidity ^0.7.6;
 GraphProxyAdmin.sol:3	 pragma solidity ^0.7.6;
 GraphProxyStorage.sol:3	 pragma solidity ^0.7.6;
 GraphUpgradeable.sol:3	 pragma solidity ^0.7.6;
```
 ### [G-7] Splitting require() statements that use && saves gas
 There is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper 

 There are 3 occurrences:
```
 L1GraphTokenGateway.sol:142	         require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
 Governed.sol:54	         require(pendingGovernor != address(0) && msg.sender == pendingGovernor,"Caller must be pending governor");
 GraphProxy.sol:142	         require(_pendingImplementation != address(0) && msg.sender == _pendingImplementation,"Caller must be the pending implementation");
```
 ### [G-8] Duplicated require()/revert() checks should be refactored to a modifier or function
 Saves deployment costs 

 There are 6 occurrences:
```
 L1GraphTokenGateway.sol:271	         require(_l1Token == address(token), "TOKEN_NOT_GRT");
 Managed.sol:49	         require(!controller.paused(), "Paused");
 L2GraphTokenGateway.sol:146	         require(_amount > 0, "INVALID_ZERO_AMOUNT");
 L2GraphTokenGateway.sol:148	         require(_to != address(0), "INVALID_DESTINATION");
 L2GraphTokenGateway.sol:233	         require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
 L2GraphTokenGateway.sol:234	         require(msg.value == 0, "INVALID_NONZERO_VALUE");
```
