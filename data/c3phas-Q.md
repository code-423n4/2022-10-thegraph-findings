## QA
### Deprecated approve() function
approve which is subject to a known front-running attack and failing for certain token implementations that do not return a boolean value. Consider using safeApprove instead (or better: safeIncreaseAllowance()/safeDecreaseAllowance()):

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/BridgeEscrow.sol#L28-L30
```solidity
File: /contracts/gateway/BridgeEscrow.sol
28:    function approveAll(address _spender) external onlyGovernor {
29:        graphToken().approve(_spender, type(uint256).max);
30:    }
```

### Return values of `approve()` not checked

Not all `IERC20` implementations `revert()` when there’s a failure in `approve()`. The function signature has a `boolean` return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually approving anything

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/BridgeEscrow.sol#L28-L30
```solidity
File: /contracts/gateway/BridgeEscrow.sol
28:    function approveAll(address _spender) external onlyGovernor {
29:        graphToken().approve(_spender, type(uint256).max);
30: 
```

### Return values of `decreaseAllowance()` not checked
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/BridgeEscrow.sol#L36-L39
```solidity
File: /contracts/gateway/BridgeEscrow.sol
36:    function revokeAll(address _spender) external onlyGovernor {
37:        IGraphToken grt = graphToken();
38:        grt.decreaseAllowance(_spender, grt.allowance(address(this), _spender));
39:    }
```

The signature of the function `decreaseAllowance()` indicates that it should return a boolean value. see the following
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/token/IGraphToken.sol#L42
```solidity
File: /contracts/token/IGraphToken.sol
42:    function decreaseAllowance(address spender, uint256 subtractedValue) external returns (bool);
```

However, our revokeAll() function does not check this return value

### Unsafe use of transfer()/transferFrom() with IERC20

Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens. For example Tether (USDT)'s transfer() and transferFrom() functions do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to IERC20, their function signatures do not match and therefore the calls made, revert. Use OpenZeppelin’s SafeERC20's safeTransfer()/safeTransferFrom() instead or check the return value.

Bad:
```solidity
IERC20(token).transferFrom(msg.sender, address(this), amount);
```
Good (using OpenZeppelin's SafeERC20):
```solidity
import {SafeERC20} from "openzeppelin/token/utils/SafeERC20.sol";

// ...

IERC20(token).safeTransferFrom(msg.sender, address(this), amount);
```
Good (using require):
```solidity
bool success = IERC20(token).transferFrom(msg.sender, address(this), amount);
require(success, "ERC20 transfer failed");
```
### Affected code
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L235
```solidity
File: /contracts/gateway/L1GraphTokenGateway.sol
235:        token.transferFrom(from, escrow, _amount);

276:        token.transferFrom(escrow, _to, _amount);

```

Consider using safeTransfer/safeTransferFrom or wrap in a require statement here:

### Missing checks for address(0x0) when assigning values to address state variables
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L55-L59
```solidity
File: /contracts/governance/Pausable.sol

//@audit: pauseGuardian = newPauseGuardian;
55:    function _setPauseGuardian(address newPauseGuardian) internal {
56:        address oldPauseGuardian = pauseGuardian;
57:        pauseGuardian = newPauseGuardian;
58:        emit NewPauseGuardian(oldPauseGuardian, pauseGuardian);
59:    }
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L31-L33
```solidity
File: /contracts/governance/Governed.sol

//@audit: governor = _initGovernor
31:    function _initialize(address _initGovernor) internal {
32:        governor = _initGovernor;
33:    }
```

### abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()

Use abi.encode() instead which will pad items to 32 bytes, which will prevent hash collisions (e.g. abi.encodePacked(0x123,0x456) => 0x123456 => abi.encodePacked(0x1,0x23456), but abi.encode(0x123,0x456) => 0x0...1230...456). "Unless there is a compelling reason, abi.encode should be preferred". If there is only one argument to abi.encodePacked() it can often be cast to bytes() or bytes32() instead.

If all arguments are strings and or bytes, bytes.concat() should be used instead


https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L83-L91
```solidity
File: /contracts/l2/token/GraphTokenUpgradeable.sol
83:        bytes32 digest = keccak256(
84:            abi.encodePacked(
85:                "\x19\x01",
86:                DOMAIN_SEPARATOR,
87:                keccak256(
88:                    abi.encode(PERMIT_TYPEHASH, _owner, _spender, _value, nonces[_owner], _deadline)
89:                )
90:            )
91:        );
```

### Non-assembly method available

`assembly{ id := chainid() }` => `uint256 id = block.chainid`

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L195-L202
```solidity
File: /contracts/l2/token/GraphTokenUpgradeable.sol
195:    function _getChainID() private pure returns (uint256) {
196:        uint256 id;
197:        // solhint-disable-next-line no-inline-assembly
198:        assembly {
199:            id := chainid()
200:        }
201:       return id;
202:    }
```

### Public functions not called by the contract should be declared external instead
Contracts are allowed to override their parents' functions and change the visibility from external to public.
Functions marked by external use call data to read arguments, where public will first allocate in local memory and then load them.

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L30
```solidity
File: /contracts/upgrades/GraphProxyAdmin.sol
30:    function getProxyImplementation(IGraphProxy _proxy) public view returns (address) {

43:    function getProxyPendingImplementation(IGraphProxy _proxy) public view returns (address) {

55:    function getProxyAdmin(IGraphProxy _proxy) public view returns (address) {

68:    function changeProxyAdmin(IGraphProxy _proxy, address _newAdmin) public onlyGovernor {

77:    function upgrade(IGraphProxy _proxy, address _implementation) public onlyGovernor {

86:    function acceptProxy(GraphUpgradeable _implementation, IGraphProxy _proxy) public onlyGovernor {

```

### Unused internal function
The following function is not being used anywhere 
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L153-L155
```solidity
File: /contracts/governance/Managed.sol
153:    function graphTokenGateway() internal view returns (ITokenGateway) {
154:        return ITokenGateway(_resolveContract(keccak256("GraphTokenGateway")));
155:    }
```
### Lock pragmas to specific compiler version
Contracts should be deployed with the same compiler version and flags that they have been tested the most with. Locking the pragma helps ensure that contracts do not accidentally get deployed using, for example, the latest compiler which may have higher risks of undiscovered bugs. Contracts may also be deployed by others and the pragma indicates the compiler version intended by the original authors.

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/BridgeEscrow.sol#L3
```solidity
File: /contracts/gateway/BridgeEscrow.sol
3:pragma solidity ^0.7.6;
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L3
```solidity
File: /contracts/upgrades/GraphUpgradeable.sol
3:pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L3
```solidity
File: /contracts/governance/Governed.sol
3:pragma solidity ^0.7.6;
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L3
```solidity
File: /contracts/governance/Pausable.sol
3:pragma solidity ^0.7.6;
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L3
```solidity
File: /contracts/l2/token/L2GraphToken.sol
3:pragma solidity ^0.7.6;
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L3
```solidity
File: /contracts/upgrades/GraphProxyAdmin.sol
3:pragma solidity ^0.7.6;
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyStorage.sol#L3
```solidity
File: /contracts/upgrades/GraphProxyStorage.sol
3:pragma solidity ^0.7.6;
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L3
```solidity
File: /contracts/upgrades/GraphProxy.sol
3:pragma solidity ^0.7.6;
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L3
```solidity
File: /contracts/governance/Managed.sol
3:pragma solidity ^0.7.6;
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L3
```solidity
File: /contracts/l2/token/GraphTokenUpgradeable.sol
3:pragma solidity ^0.7.6;
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L3
```solidity
File: /contracts/l2/gateway/L2GraphTokenGateway.sol
3:pragma solidity ^0.7.6;
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L3
```solidity
File: /contracts/gateway/L1GraphTokenGateway.sol
3:pragma solidity ^0.7.6;
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/GraphTokenGateway.sol#L3
```solidity
File: /contracts/gateway/GraphTokenGateway.sol
3:pragma solidity ^0.7.6;
```

### Uppercase variable Names should be reserved for constants
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L50
```solidity
File: /contracts/l2/token/GraphTokenUpgradeable.sol
50:    bytes32 private DOMAIN_SEPARATOR;
```

### Natspec is incomplete
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L104
```solidity
File: /contracts/upgrades/GraphProxy.sol

//@audit: Missing @param _newAdmin
104:    function setAdmin(address _newAdmin) external ifAdmin {

//@audit: Missing @param data
129:    function acceptUpgradeAndCall(bytes calldata data) external ifAdminOrPendingImpl {
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L30
```solidity
File: /contracts/upgrades/GraphProxyAdmin.sol

//@audit: Missing @param _proxy
30:    function getProxyImplementation(IGraphProxy _proxy) public view returns (address) {

//@audit: Missing @param _proxy
43:    function getProxyPendingImplementation(IGraphProxy _proxy) public view returns (address) {

//@audit: Missing @param _proxy
55:    function getProxyAdmin(IGraphProxy _proxy) public view returns (address) {

```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L50
```solidity
File: /contracts/upgrades/GraphUpgradeable.sol

//@audit: Missing @param _proxy
50:    function acceptProxy(IGraphProxy _proxy) external onlyProxyAdmin(_proxy) {

//@audit: Missing @param _proxy, @param _data
59:    function acceptProxyAndCall(IGraphProxy _proxy, bytes calldata _data)
```

### Lack of event emission after critical initialize() functions
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L31-L33
```solidity
File: /contracts/governance/Governed.sol
31:    function _initialize(address _initGovernor) internal {
32:        governor = _initGovernor;
33:    }
```
### Missing friendly revert strings
 require()/revert() statements should have descriptive reason strings.
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L34
```solidity
File: /contracts/upgrades/GraphProxyAdmin.sol
34:        require(success);

47:        require(success);

59:        require(success);


```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L133
```solidity
File: /contracts/upgrades/GraphProxy.sol
133:        require(success);
```

### The nonReentrant modifier should occur before all other modifiers
This is a best-practice to protect against reentrancy in other modifiers

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L137-L144
```solidity
File: /contracts/l2/gateway/L2GraphTokenGateway.sol
137:    function outboundTransfer(
138:        address _l1Token,
139:        address _to,
140:        uint256 _amount,
141:        uint256, // unused on L2
142:        uint256, // unused on L2
143:        bytes calldata _data
144:    ) public payable override notPaused nonReentrant returns (bytes memory) {
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L226-L232
```solidity
File: /contracts/l2/gateway/L2GraphTokenGateway.sol
226:    function finalizeInboundTransfer(
227:        address _l1Token,
228:        address _from,
229:        address _to,
230:        uint256 _amount,
231:        bytes calldata _data
232:    ) external payable override notPaused onlyL1Counterpart nonReentrant {
```
