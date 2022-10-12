##  Using `bool`s for storage incurs overhead 
    
Use `uint256(1)`and `uint256(2)`for true/false to avoid a Gwarmaccess (**[100 gas](https://gist.github.com/IllIllI000/1b70014db712f8572a72378321250058)**) for the extra SLOAD, and to avoid Gsset (**20000 gas**) when changing from `false`to `true`, after having been `true`in the past

    File: contracts/governance/Pausable.sol   #1
    8    bool internal _partialPaused;

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L8

      File: contracts/governance/Pausable.sol   #2
      10    bool internal _paused;

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L10

      File: contracts/gateway/L1GraphTokenGateway.sol   #3
      35    mapping(address => bool) public callhookWhitelist;

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L35

## Multiple `address` mappings can be combined into a single mapping of an `address` to a `struct`, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot

    File: contracts/l2/token/GraphTokenUpgradeable.sol   #1
    51    mapping(address => bool) private _minters;
    52    mapping(address => uint256) public nonces;

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L51-L52

## `REQUIRE()` STATEMENTS SHOULD HAVE DESCRIPTIVE REASON STRINGS

    File: contracts/upgrades/GraphProxyAdmin.sol   #1
    47        require(success);

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L47

      File: contracts/upgrades/GraphProxyAdmin.sol   #2
      34        require(success);
      
* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L34

##  Using `> 0` costs more gas than `!= 0` when used on a `uint` in a `require()` statement
    
This change saves [6 gas](https://aws1.discourse-cdn.com/business6/uploads/zeppelin/original/2X/3/363a367d6d68851f27d2679d10706cd16d788b96.png) per instance

    File: contracts/l2/gateway/L2GraphTokenGateway.sol   #1
    146        require(_amount > 0, "INVALID_ZERO_AMOUNT");

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L146

      File: contracts/gateway/L1GraphTokenGateway.sol   #2
      201         require(_amount > 0, "INVALID_ZERO_AMOUNT");

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L201

      File: contracts/gateway/L1GraphTokenGateway.sol   #3
      201           require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L217

##  Splitting `require()` statements that use `&&` saves gas
    
Instead of using the `&&` operator in a single require statement to check multiple conditions, I suggest using multiple require statements with 1 condition per require statement (saving 3 gas per `&`):

    File: contracts/governance/Governed.sol   #1
    55            pendingGovernor != address(0) && msg.sender == pendingGovernor,

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L55

      File: contracts/upgrades/GraphProxy.sol   #2
      143            _pendingImplementation != address(0) && msg.sender == _pendingImplementation,

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L143

      File: contracts/gateway/L1GraphTokenGateway.sol   #3
      142         require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L142
 
## `abi.encode()` is less efficient than `abi.encodePacked()`

    File: contracts/l2/token/GraphTokenUpgradeable.sol   #1
    88                    abi.encode(PERMIT_TYPEHASH, _owner, _spender, _value, nonces[_owner], _deadline)

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L88

      File: contracts/l2/token/GraphTokenUpgradeable.sol   #2
      162            abi.encode(

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L162

      File: contracts/l2/gateway/L2GraphTokenGateway.sol   #3
      174        return abi.encode(id);

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L174

##  `Inline` a modifier that's only used once
    
As `onlyGovernor()`is only used once in this contract (in `function transferOwnership()`), it should get inlined to save gas without losing readability:

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L23

As `onlyMinter()`is only used once in this contract (in `function mint()`), it should get inlined to save gas without losing readability:

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L59

## Use custom errors rather than `revert()`/`require()` strings to save deployment gas
    
Custom errors are available from solidity version 0.8.4. 
Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met).
    
Custom errors save ~50 gas each time they’re hit by avoiding having to allocate and store the revert string. Not defining the strings also save

    For example:         File: contracts/governance/Governed.sol
    24        require(msg.sender == governor, "Only Governor can call");
 
* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L24

      Below instances are also affected :

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L41

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L56

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L36

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L49

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L60

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L70

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L62

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L141

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L106

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L115
