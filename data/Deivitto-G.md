# GAS
## Constants expressions are expressions, not constants.
### Summary
Constant expressions are left as expressions, not constants.
### Details
Reference to this kind of issue: https://github.com/ethereum/solidity/issues/9232
### Github Permalinks
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/token/GraphTokenUpgradeable.sol#L34-L39
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/token/GraphTokenUpgradeable.sol#L41-L45
### Mitigation
Use immutable for this expressions



## use `!=` rather than `>0` for unsigned integers in `require()` statements
### Summary
When the optimizer is enabled, gas is wasted by doing a greater-than operation, rather than a not-equals operation inside `require()` statements. When Using `!=` , the optimizer is able to avoid the `EQ`, `ISZERO`, and associated operations, by relying on the `JUMPI` that comes afterwards, which itself checks for zero.
### Github Permalinks
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/gateway/L2GraphTokenGateway.sol#L146
        require(_amount > 0, "INVALID_ZERO_AMOUNT");

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/L1GraphTokenGateway.sol#L201
        require(_amount > 0, "INVALID_ZERO_AMOUNT");

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/L1GraphTokenGateway.sol#L217
                require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");

### Mitigation
Use `!= 0` rather than `> 0` for unsigned integers in `require()` statements.

## splitting `require()` statements that use `&&` saves gas
### Summary
Instead of using the && operator in a single require statement to check multiple conditions, consider using multiple require statements with 1 condition per require statement (saving 3 gas per & ):
### Github Permalinks
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/governance/Governed.sol#L55
            pendingGovernor != address(0) && msg.sender == pendingGovernor,

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxy.sol#L143
            _pendingImplementation != address(0) && msg.sender == _pendingImplementation,

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/gateway/L1GraphTokenGateway.sol#L142
        require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");

### Mitigation
Split require statements


## Reduce the size of error messages (Long revert Strings)
### Summary
Shortening revert strings to fit in 32 bytes will decrease deployment time gas and will decrease runtime gas when the revert condition is met.
Revert strings that are longer than 32 bytes require at least one additional mstore, along with additional overhead for computing memory offset, etc.

### Github Permalinks
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphUpgradeable.sol#L32
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxy.sol#L105
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxy.sol#L141
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxy.sol#L144
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/governance/Managed.sol#L53

### Mitigation
Consider shortening the revert strings to fit in 32 bytes


## Using bools for storage incurs overhead { 
### Summary
Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

### Details
Here is one example of OpenZeppelin about this optimization
https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27 
Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past

### Github Permalinks
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/governance/Pausable.sol#L8
    bool internal _partialPaused;

https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/governance/Pausable.sol#L10
    bool internal _paused;
### Mitigation
Consider using uint256 with values 0 and 1 rather than bool


## Store using Struct over multiple mappings
### Summary
All these variables could be combine in a Struct in order to reduce the gas cost. 
### Details
As noticed in: 
https://gist.github.com/alexon1234/b101e3ac51bea3cbd9cf06f80eaa5bc2
When multiple mappings that access the same addresses, uints, etc, all of them can be mixed into an struct and then that data accessed like:
mapping(datatype => newStructCreated) newStructMap;
Also, you have this post where it explains the benefits of using Structs over mappings 
https://medium.com/@novablitz/storing-structs-is-costing-you-gas-774da988895e

### Github Permalinks
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/token/GraphTokenUpgradeable.sol#L51-L52
- - - 

### Mitigation
Consider mixing different mappings into an struct when able in order to save gas.



## `>=` cheaper than `>`
### Summary
Strict inequalities ( `>` ) are more expensive than non-strict ones ( `>=` ). This is due to some supplementary checks (`ISZERO`, 3 gas)

### Github Permalinks
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/gateway/L2GraphTokenGateway.sol#L238
        if (_data.length > 0) {

### Mitigation
Consider using `>= 1` instead of `> 0` to avoid some opcodes


## Unnecesary storage read
### Summary
A value which value is already known can be used directly rather than reading it from the storage
### Example
```
    function acceptOwnership() external {
        require(
            pendingGovernor != address(0) && msg.sender == pendingGovernor,
            "Caller must be pending governor"
        );

        address oldGovernor = governor;
        address oldPendingGovernor = pendingGovernor;

        governor = pendingGovernor;
        pendingGovernor = address(0);

        emit NewOwnership(oldGovernor, governor);
        emit NewPendingOwnership(oldPendingGovernor, pendingGovernor);
    }
```

Recommendation
Change to:

```
    function acceptOwnership() external {
        require(
            pendingGovernor != address(0) && msg.sender == pendingGovernor,
            "Caller must be pending governor"
        );

        address oldGovernor = governor;
        address oldPendingGovernor = pendingGovernor;

        governor = pendingGovernor;
        pendingGovernor = address(0);

        emit NewOwnership(oldGovernor, oldPendingGovernor);
        emit NewPendingOwnership(oldPendingGovernor, address(0));
    }
```
### Github Permalinks
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/governance/Governed.sol#L53-L67

- Same happens:
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/governance/Governed.sol#L40-L47
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/governance/Pausable.sol#L26-L35
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/governance/Pausable.sol#L40-L49
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/governance/Pausable.sol#L58
https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/l2/token/L2GraphToken.sol#L59-L62
### Mitigation
Set directly the value to avoid unnecessary storage read to save some gas

