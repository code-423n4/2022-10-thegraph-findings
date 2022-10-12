### [G-01] ```abi.encode()``` is less efficient than ```abi.encodePacked()```


#### Impact
Consider using ```abi.encodePacked()``` instead for efficieny.


#### Findings:
```
contracts/gateway/L1GraphTokenGateway.sol:L249        return abi.encode(seqNum);

contracts/gateway/L1GraphTokenGateway.sol:L342                abi.encode(emptyBytes, _data)

contracts/l2/token/GraphTokenUpgradeable.sol:L88                    abi.encode(PERMIT_TYPEHASH, _owner, _spender, _value, nonces[_owner], _deadline)

contracts/l2/token/GraphTokenUpgradeable.sol:L162            abi.encode(

contracts/l2/gateway/L2GraphTokenGateway.sol:L174        return abi.encode(id);

contracts/l2/gateway/L2GraphTokenGateway.sol:L275                abi.encode(0, _data) // we don't need to track exitNums (b/c we have no fast exits) so we always use 0

```

### [G-02] Use assembly to check for zero address.


#### Impact
Save 6 gas when assembly is used to check for zero address.
e.g:
```
assembly {
            if iszero(_addr) {
                mstore(0x00, "zero address")
                revert(0x00, 0x20)
            }
```


#### Findings:
```
contracts/upgrades/GraphProxy.sol:L105        require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");

```

### [G-03] Using bools for storage incurs overhead.


#### Impact
 
```
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.
```
https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27 Use ```uint256(1)``` and ```uint256(2)``` for true/false to avoid a Gwarmaccess ([100 gas](https://gist.github.com/IllIllI000/1b70014db712f8572a72378321250058)), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past


#### Findings:
```
contracts/gateway/L1GraphTokenGateway.sol:L35    mapping(address => bool) public callhookWhitelist;

contracts/l2/token/GraphTokenUpgradeable.sol:L51    mapping(address => bool) private _minters;

```

### [G-04] Use custom errors rather than ```revert()```/```require()``` string to save gas


#### Impact
Custom errors are available from solidity version 0.8.4, it saves around 50 gas each time they are hit by avoiding having to allocate and store the revert string.


#### Findings:
```
contracts/gateway/L1GraphTokenGateway.sol:L74        require(inbox != address(0), "INBOX_NOT_SET");

contracts/gateway/L1GraphTokenGateway.sol:L78        require(msg.sender == address(bridge), "NOT_FROM_BRIDGE");

contracts/gateway/L1GraphTokenGateway.sol:L82        require(l2ToL1Sender == l2Counterpart, "ONLY_COUNTERPART_GATEWAY");

contracts/gateway/L1GraphTokenGateway.sol:L110        require(_inbox != address(0), "INVALID_INBOX");

contracts/gateway/L1GraphTokenGateway.sol:L111        require(_l1Router != address(0), "INVALID_L1_ROUTER");

contracts/gateway/L1GraphTokenGateway.sol:L122        require(_l2GRT != address(0), "INVALID_L2_GRT");

contracts/gateway/L1GraphTokenGateway.sol:L132        require(_l2Counterpart != address(0), "INVALID_L2_COUNTERPART");

contracts/gateway/L1GraphTokenGateway.sol:L142        require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");

contracts/gateway/L1GraphTokenGateway.sol:L153        require(_newWhitelisted != address(0), "INVALID_ADDRESS");

contracts/gateway/L1GraphTokenGateway.sol:L154        require(!callhookWhitelist[_newWhitelisted], "ALREADY_WHITELISTED");

contracts/gateway/L1GraphTokenGateway.sol:L165        require(_notWhitelisted != address(0), "INVALID_ADDRESS");

contracts/gateway/L1GraphTokenGateway.sol:L166        require(callhookWhitelist[_notWhitelisted], "NOT_WHITELISTED");

contracts/gateway/L1GraphTokenGateway.sol:L200        require(_l1Token == address(token), "TOKEN_NOT_GRT");

contracts/gateway/L1GraphTokenGateway.sol:L201        require(_amount > 0, "INVALID_ZERO_AMOUNT");

contracts/gateway/L1GraphTokenGateway.sol:L202        require(_to != address(0), "INVALID_DESTINATION");

contracts/gateway/L1GraphTokenGateway.sol:L217                require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");

contracts/gateway/L1GraphTokenGateway.sol:L224                    require(msg.value >= expectedEth, "WRONG_ETH_VALUE");

contracts/gateway/L1GraphTokenGateway.sol:L271        require(_l1Token == address(token), "TOKEN_NOT_GRT");

contracts/gateway/L1GraphTokenGateway.sol:L275        require(_amount <= escrowBalance, "BRIDGE_OUT_OF_FUNDS");

contracts/gateway/GraphTokenGateway.sol:L31        require(_newPauseGuardian != address(0), "PauseGuardian must be set");

contracts/gateway/GraphTokenGateway.sol:L40        require(!_paused, "Paused (contract)");

contracts/upgrades/GraphProxy.sol:L105        require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");

contracts/upgrades/GraphProxy.sol:L141        require(Address.isContract(_pendingImplementation), "Implementation must be a contract");

contracts/upgrades/GraphProxy.sol:L157        require(msg.sender != _admin(), "Cannot fallback to proxy target");

contracts/upgrades/GraphUpgradeable.sol:L24        require(msg.sender == _proxy.admin(), "Caller must be the proxy admin");

contracts/upgrades/GraphUpgradeable.sol:L32        require(msg.sender == _implementation(), "Caller must be the implementation");

contracts/upgrades/GraphProxyStorage.sol:L62        require(msg.sender == _admin(), "Caller must be admin");

contracts/governance/Governed.sol:L24        require(msg.sender == governor, "Only Governor can call");

contracts/governance/Governed.sol:L41        require(_newGovernor != address(0), "Governor must be set");

contracts/governance/Managed.sol:L44        require(!controller.paused(), "Paused");

contracts/governance/Managed.sol:L45        require(!controller.partialPaused(), "Partial-paused");

contracts/governance/Managed.sol:L49        require(!controller.paused(), "Paused");

contracts/governance/Managed.sol:L53        require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");

contracts/governance/Managed.sol:L57        require(msg.sender == address(controller), "Caller must be Controller");

contracts/governance/Managed.sol:L104        require(_controller != address(0), "Controller must be set");

contracts/l2/token/L2GraphToken.sol:L36        require(msg.sender == gateway, "NOT_GATEWAY");

contracts/l2/token/L2GraphToken.sol:L49        require(_owner != address(0), "Owner must be set");

contracts/l2/token/L2GraphToken.sol:L60        require(_gw != address(0), "INVALID_GATEWAY");

contracts/l2/token/L2GraphToken.sol:L70        require(_addr != address(0), "INVALID_L1_ADDRESS");

contracts/l2/token/GraphTokenUpgradeable.sol:L60        require(isMinter(msg.sender), "Only minter can call");

contracts/l2/token/GraphTokenUpgradeable.sol:L94        require(_owner == recoveredAddress, "GRT: invalid permit");

contracts/l2/token/GraphTokenUpgradeable.sol:L95        require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");

contracts/l2/token/GraphTokenUpgradeable.sol:L106        require(_account != address(0), "INVALID_MINTER");

contracts/l2/token/GraphTokenUpgradeable.sol:L115        require(_minters[_account], "NOT_A_MINTER");

contracts/l2/token/GraphTokenUpgradeable.sol:L123        require(_minters[msg.sender], "NOT_A_MINTER");

contracts/l2/gateway/L2GraphTokenGateway.sol:L98        require(_l2Router != address(0), "INVALID_L2_ROUTER");

contracts/l2/gateway/L2GraphTokenGateway.sol:L108        require(_l1GRT != address(0), "INVALID_L1_GRT");

contracts/l2/gateway/L2GraphTokenGateway.sol:L118        require(_l1Counterpart != address(0), "INVALID_L1_COUNTERPART");

contracts/l2/gateway/L2GraphTokenGateway.sol:L145        require(_l1Token == l1GRT, "TOKEN_NOT_GRT");

contracts/l2/gateway/L2GraphTokenGateway.sol:L146        require(_amount > 0, "INVALID_ZERO_AMOUNT");

contracts/l2/gateway/L2GraphTokenGateway.sol:L147        require(msg.value == 0, "INVALID_NONZERO_VALUE");

contracts/l2/gateway/L2GraphTokenGateway.sol:L148        require(_to != address(0), "INVALID_DESTINATION");

contracts/l2/gateway/L2GraphTokenGateway.sol:L153        require(outboundCalldata.extraData.length == 0, "CALL_HOOK_DATA_NOT_ALLOWED");

contracts/l2/gateway/L2GraphTokenGateway.sol:L233        require(_l1Token == l1GRT, "TOKEN_NOT_GRT");

contracts/l2/gateway/L2GraphTokenGateway.sol:L234        require(msg.value == 0, "INVALID_NONZERO_VALUE");

```
### [G-05] Public functions not called by the contract should be declared external instead


#### Impact
public functions that are never called by the contract should be declared external to save gas.


#### Findings:
```
contracts/upgrades/GraphProxyAdmin.sol:L68    function changeProxyAdmin(IGraphProxy _proxy, address _newAdmin) public onlyGovernor {

contracts/upgrades/GraphProxyAdmin.sol:L77    function upgrade(IGraphProxy _proxy, address _implementation) public onlyGovernor {

```

### [G-06] Using ```> 0``` costs more gas than ```!= 0``` when used on a uint in a ```require()``` statement.


#### Impact
When working with unsigned integer types, comparisons with != 0 are cheaper than with > 0 . This changes saves 6 gas per instance.


#### Findings:
```
contracts/gateway/L1GraphTokenGateway.sol:L201        require(_amount > 0, "INVALID_ZERO_AMOUNT");

contracts/gateway/L1GraphTokenGateway.sol:L217                require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");

contracts/l2/gateway/L2GraphTokenGateway.sol:L146        require(_amount > 0, "INVALID_ZERO_AMOUNT");

```

### [G-07] Use a more recent version of solidity.


#### Impact
Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining 
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads 
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings 
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value.


#### Findings:
```
contracts/token/IGraphToken.sol:L3      pragma solidity ^0.7.6;

contracts/staking/IStakingData.sol:L3      pragma solidity >=0.6.12 <0.8.0;

contracts/staking/IStaking.sol:L3      pragma solidity >=0.6.12 <0.8.0;

contracts/rewards/IRewardsManager.sol:L3      pragma solidity ^0.7.6;

contracts/gateway/L1GraphTokenGateway.sol:L3      pragma solidity ^0.7.6;

contracts/gateway/ICallhookReceiver.sol:L9      pragma solidity ^0.7.6;

contracts/gateway/GraphTokenGateway.sol:L3      pragma solidity ^0.7.6;

contracts/gateway/BridgeEscrow.sol:L3      pragma solidity ^0.7.6;

contracts/upgrades/GraphProxy.sol:L3      pragma solidity ^0.7.6;

contracts/upgrades/GraphUpgradeable.sol:L3      pragma solidity ^0.7.6;

contracts/upgrades/IGraphProxy.sol:L3      pragma solidity ^0.7.6;

contracts/upgrades/GraphProxyAdmin.sol:L3      pragma solidity ^0.7.6;

contracts/upgrades/GraphProxyStorage.sol:L3      pragma solidity ^0.7.6;

contracts/governance/Governed.sol:L3      pragma solidity ^0.7.6;

contracts/governance/Pausable.sol:L3      pragma solidity ^0.7.6;

contracts/governance/IController.sol:L3      pragma solidity >=0.6.12 <0.8.0;

contracts/governance/Managed.sol:L3      pragma solidity ^0.7.6;

contracts/epochs/IEpochManager.sol:L3      pragma solidity ^0.7.6;

contracts/curation/IGraphCurationToken.sol:L3      pragma solidity ^0.7.6;

contracts/curation/ICuration.sol:L3      pragma solidity ^0.7.6;

contracts/l2/token/L2GraphToken.sol:L3      pragma solidity ^0.7.6;

contracts/l2/token/GraphTokenUpgradeable.sol:L3      pragma solidity ^0.7.6;

contracts/l2/gateway/L2GraphTokenGateway.sol:L3      pragma solidity ^0.7.6;

```

### [G-08] Functions guaranteed to revert when called by normal users can be declared as payable.


#### Impact
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.


#### Findings:
```
contracts/gateway/L1GraphTokenGateway.sol:L99    function initialize(address _controller) external onlyImpl {

contracts/gateway/L1GraphTokenGateway.sol:L109    function setArbitrumAddresses(address _inbox, address _l1Router) external onlyGovernor {

contracts/gateway/L1GraphTokenGateway.sol:L121    function setL2TokenAddress(address _l2GRT) external onlyGovernor {

contracts/gateway/L1GraphTokenGateway.sol:L131    function setL2CounterpartAddress(address _l2Counterpart) external onlyGovernor {

contracts/gateway/L1GraphTokenGateway.sol:L141    function setEscrowAddress(address _escrow) external onlyGovernor {

contracts/gateway/L1GraphTokenGateway.sol:L152    function addToCallhookWhitelist(address _newWhitelisted) external onlyGovernor {

contracts/gateway/L1GraphTokenGateway.sol:L164    function removeFromCallhookWhitelist(address _notWhitelisted) external onlyGovernor {

contracts/gateway/GraphTokenGateway.sol:L30    function setPauseGuardian(address _newPauseGuardian) external onlyGovernor {

contracts/gateway/GraphTokenGateway.sol:L47    function setPaused(bool _newPaused) external onlyGovernorOrGuardian {

contracts/gateway/BridgeEscrow.sol:L20    function initialize(address _controller) external onlyImpl {

contracts/gateway/BridgeEscrow.sol:L28    function approveAll(address _spender) external onlyGovernor {

contracts/gateway/BridgeEscrow.sol:L36    function revokeAll(address _spender) external onlyGovernor {

contracts/upgrades/GraphUpgradeable.sol:L50    function acceptProxy(IGraphProxy _proxy) external onlyProxyAdmin(_proxy) {

contracts/upgrades/GraphProxyAdmin.sol:L68    function changeProxyAdmin(IGraphProxy _proxy, address _newAdmin) public onlyGovernor {

contracts/upgrades/GraphProxyAdmin.sol:L77    function upgrade(IGraphProxy _proxy, address _implementation) public onlyGovernor {

contracts/upgrades/GraphProxyAdmin.sol:L86    function acceptProxy(GraphUpgradeable _implementation, IGraphProxy _proxy) public onlyGovernor {

contracts/governance/Governed.sol:L40    function transferOwnership(address _newGovernor) external onlyGovernor {

contracts/governance/Managed.sol:L95    function setController(address _controller) external onlyController {

contracts/l2/token/L2GraphToken.sol:L48    function initialize(address _owner) external onlyImpl {

contracts/l2/token/L2GraphToken.sol:L59    function setGateway(address _gw) external onlyGovernor {

contracts/l2/token/L2GraphToken.sol:L69    function setL1Address(address _addr) external onlyGovernor {

contracts/l2/token/L2GraphToken.sol:L80    function bridgeMint(address _account, uint256 _amount) external override onlyGateway {

contracts/l2/token/L2GraphToken.sol:L90    function bridgeBurn(address _account, uint256 _amount) external override onlyGateway {

contracts/l2/token/GraphTokenUpgradeable.sol:L105    function addMinter(address _account) external onlyGovernor {

contracts/l2/token/GraphTokenUpgradeable.sol:L114    function removeMinter(address _account) external onlyGovernor {

contracts/l2/token/GraphTokenUpgradeable.sol:L132    function mint(address _to, uint256 _amount) external onlyMinter {

contracts/l2/gateway/L2GraphTokenGateway.sol:L87    function initialize(address _controller) external onlyImpl {

contracts/l2/gateway/L2GraphTokenGateway.sol:L97    function setL2Router(address _l2Router) external onlyGovernor {

contracts/l2/gateway/L2GraphTokenGateway.sol:L107    function setL1TokenAddress(address _l1GRT) external onlyGovernor {

contracts/l2/gateway/L2GraphTokenGateway.sol:L117    function setL1CounterpartAddress(address _l1Counterpart) external onlyGovernor {

```

### [G-09] Revert message greater than 32 bytes


#### Impact
Keep revert message lower than or equal to 32 bytes to save gas.


#### Findings:
```
contracts/gateway/GraphTokenGateway.sol:L19        require(

contracts/upgrades/GraphProxy.sol:L105        require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");

contracts/upgrades/GraphProxy.sol:L141        require(Address.isContract(_pendingImplementation), "Implementation must be a contract");

contracts/upgrades/GraphProxy.sol:L142        require(

contracts/upgrades/GraphUpgradeable.sol:L32        require(msg.sender == _implementation(), "Caller must be the implementation");

contracts/governance/Managed.sol:L53        require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");

```

### [G-10] Splitting ```require()``` statements that use && saves gas.


#### Impact
Consider splitting the ```require()``` statements to save gas.


#### Findings:
```
contracts/gateway/L1GraphTokenGateway.sol:L142        require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");

```

