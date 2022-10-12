### `Require` revert string is too long
The revert strings below can be shortened to 32 characters or fewer (as shown) to save gas (or else consider using custom error codes, which could save even more gas)
___
[GraphUpgradeable.sol: L32](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L32)
```solidity
        require(msg.sender == _implementation(), "Caller must be the implementation");
```
Change message to `Caller must be the impl`
___
[GraphProxy.sol: L141](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L141)
```solidity
        require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
```
Change message to `Implementation must be contract`
___
[GraphProxy.sol: L142-145](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L142-L145)
```solidity
        require(
            _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
            "Caller must be the pending implementation"
        );
```
Change message to `Caller must be the pending impl`
___
[Managed.sol: L53](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L53)
```solidity
        require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
```
Change message to `Caller must be Controller gov`
___
[GraphTokenGateway.sol: L21](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/GraphTokenGateway.sol#L21)
```solidity
            "Only Governor or Guardian can call"
```
Change message to `Only Gov or Guardian can call`
___

The revert string below is also longer than 32 characters but it's not clear how to shorten it

[GraphProxy.sol: L105](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L105)
```solidity
        require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
```
___
___

### Avoid use of '&&' within a `require` function
Splitting into separate `require()` statements saves gas
___
[Governed.sol: L54-57](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L54-L57)
```solidity
        require(
            pendingGovernor != address(0) && msg.sender == pendingGovernor,
            "Caller must be pending governor"
        );
```
Recommendation:
```solidity
        require(pendingGovernor != address(0), "Caller must be pending governor");
        require(msg.sender == pendingGovernor, "Caller must be pending governor");
```
___
[GraphProxy.sol: L142-145](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L142-L145)
```solidity
        require(
            _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
            "Caller must be the pending implementation"
        );
```
Recommendation:
```solidity
        require(_pendingImplementation != address(0), "Caller must be the pending impl");
        require(msg.sender == _pendingImplementation, "Caller must be the pending impl");
```
___
[L1GraphTokenGateway.sol: L142](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L142)
```solidity
        require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
```
Recommendation:
```solidity
        require(_escrow != address(0), "Invalid escrow");
        require(Address.isContract(_escrow), "Invalid escrow");
```
___
___

### Use `!= 0` instead of `> 0` in a `require` statement if variable is an unsigned integer
`!= 0` should be used where possible since `> 0` costs more gas
___
[L1GraphTokenGateway.sol: L217](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L217)
```solidity
                require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
```
Change `maxSubmissionCost > 0` to `maxSubmissionCost != 0`  
___
___

### Inline a function/modifier that’s only used once
It costs less gas to inline the code of functions that are only called once. Doing so saves the cost of allocating the function selector, and the cost of the jump when the function is called.
___
[Managed.sol: L70-74](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L70-L74)
```solidity
    // Check if sender is controller.
    modifier onlyController() {
        _onlyController();
        _;
    }
```
Since `onlyController()` is used only once in this contract (in `function setController()`) as shown below, it should be inlined to save gas

[Managed.sol: L95-97](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L95-L97)
```solidity
    function setController(address _controller) external onlyController {
        _setController(_controller);
    }
```
___
[GraphTokenUpgradeable.sol: L59-62](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L59-L62)
```solidity
    modifier onlyMinter() {
        require(isMinter(msg.sender), "Only minter can call");
        _;
    }
```
Since `onlyMinter()` is used only once in this contract (in `function mint()`) as shown below, it should be inlined to save gas

[GraphTokenUpgradeable.sol: L132-134](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L132-L134)
```solidity
    function mint(address _to, uint256 _amount) external onlyMinter {
        _mint(_to, _amount);
    }
```
___
[L2GraphTokenGateway.sol: L68-74](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L68-L74)
```solidity
    modifier onlyL1Counterpart() {
        require(
            msg.sender == AddressAliasHelper.applyL1ToL2Alias(l1Counterpart),
            "ONLY_COUNTERPART_GATEWAY"
        );
        _;
    }
```
Since `onlyL1Counterpart()` is used only once in this contract (in `function finalizeInboundTransfer()`) as shown below, it should be inlined to save gas

[L2GraphTokenGateway.sol: L226-232](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L226-L232)
```solidity
    function finalizeInboundTransfer(
        address _l1Token,
        address _from,
        address _to,
        uint256 _amount,
        bytes calldata _data
    ) external payable override notPaused onlyL1Counterpart nonReentrant {
```
___
[L1GraphTokenGateway.sol: L73-74](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L73-L74)
```solidity
    modifier onlyL2Counterpart() {
        require(inbox != address(0), "INBOX_NOT_SET");
```
Since `onlyL2Counterpart()` is used only once in this contract (in `function finalizeInboundTransfer()`) as shown below, it should be inlined to save gas

[L1GraphTokenGateway.sol: L263-269](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L263-L269)
```solidity
    function finalizeInboundTransfer(
        address _l1Token,
        address _from,
        address _to,
        uint256 _amount,
        bytes calldata // _data, contains exitNum, unused by this contract
    ) external payable override notPaused onlyL2Counterpart {
```
___
[GraphTokenGateway.sol: L18-22](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/GraphTokenGateway.sol#L18-L22)
```solidity
    modifier onlyGovernorOrGuardian() {
        require(
            msg.sender == controller.getGovernor() || msg.sender == pauseGuardian,
            "Only Governor or Guardian can call"
        );
```
Since `onlyGovernorOrGuardian()` is used only once in this contract (in `function setPaused()`) as shown below, it should be inlined to save gas

[GraphTokenGateway.sol: L47-49](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/GraphTokenGateway.sol#L47-L49)
```solidity
    function setPaused(bool _newPaused) external onlyGovernorOrGuardian {
        _setPaused(_newPaused);
    }
```
___

The following two modifiers are never used and should be reviewed:

[GraphProxyStorage.sol: L59-64](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyStorage.sol#L59-L64)
```solidity
     * @dev Modifier to check whether the `msg.sender` is the admin.
     */
    modifier onlyAdmin() {
        require(msg.sender == _admin(), "Caller must be admin");
        _;
    }
```
___
[Managed.sol: L60-63](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L60-L63)
```solidity
    modifier notPartialPaused() {
        _notPartialPaused();
        _;
    }
```
___
___
