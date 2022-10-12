Gas report (12 findings with 27 instances )

## 1. Upgrading the solc compiler to >=0.8 may save gas 
Contracts  allow use of solc version 0.7.6, which is fairly dated. Upgrading the solc compiler to 0.8 or higher will give the latest compiler benefits including bug fixes, security enhancements and overall optimizations especially the in-built overflow/underflow checks which may give gas savings by avoiding expensive SafeMath.
```
pragma solidity ^0.7.6;
```
In all scope files.

## 2. Change  encode to encodePacked
In the contracts, change abi.encode to abi.encodePacked can save gas.
*There are 4 instances of this issue:*
```
return abi.encode(seqNum);
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L249
```
abi.encode(emptyBytes, _data)
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L342
```
return abi.encode(id);
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L174

```
abi.encode(0, _data)
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L275


## 3. Splitting require() statements that use && saves gas
See [this issue](https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper
There are 3 instances of this issue:
```
require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L142

```
require(
            pendingGovernor != address(0) && msg.sender == pendingGovernor,
            "Caller must be pending governor"
        );
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L55-L57

```
require(
            _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
            "Caller must be the pending implementation"
        );
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L142-L145


## 4. require()/revert() strings longer than 32 bytes cost extra gas
Each extra memory word of bytes past the original 32 [incurs an MSTORE] (https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#consider-having-short-revert-strings) which costs 3 gas
There are 4 instances of this issue:
```
require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L53

```
require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L105
```
require(
            _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
            "Caller must be the pending implementation"
        );
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L142-L145
```
require(msg.sender == _implementation(), "Caller must be the implementation");
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L32

## 5. Consider using custom errors instead of revert strings
Solidity 0.8.4 introduced [custom errors](https://blog.soliditylang.org/2021/04/21/custom-errors/). They are more gas efficient than revert strings, when it comes to deploy cost as well as runtime cost when the revert condition is met. Use custom errors instead of revert strings for gas savings.

## 6. Avoid use of state variables in event emissions to save gas
Where possible, use equivalent function parameters or local variables in event emits instead of state variables to prevent expensive SLOADs. Post-Berlin, SLOADs on state variables accessed first-time in a transaction increased from 800 gas to 2100, which is a 2.5x increase.
```
function setGateway(address _gw) external onlyGovernor {
        require(_gw != address(0), "INVALID_GATEWAY");
        gateway = _gw;
        emit GatewaySet(gateway);
    }

```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L62

## 7. > 0 can be replaced with != 0 for gas optimisation
*There are 4 instances of this issue:* 
```
require(_amount > 0, "INVALID_ZERO_AMOUNT");

```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L201
```
require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L217
```
require(_amount > 0, "INVALID_ZERO_AMOUNT");
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L146
```
if (_data.length > 0) {
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L238

## 8. Use ERC-165 instead of homebrew staticcall-based check
The more widespread pattern is using the ERC-165 interface, which not only checks a single functions, but a complete interface. The staticcall approach has the possibility of wasting gas, should the recipient perform a lot of steps before hitting an exception.

```  
function getProxyImplementation(IGraphProxy _proxy) public view returns (address) {
        // We need to manually run the static call since the getter cannot be flagged as view
        // bytes4(keccak256("implementation()")) == 0x5c60da1b
        (bool success, bytes memory returndata) = address(_proxy).staticcall(hex"5c60da1b");
        require(success);
        return abi.decode(returndata, (address));
    }
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L30-L61

## 9. inbox SHOULD GET CACHED
```
modifier onlyL2Counterpart() {
        require(inbox != address(0), "INBOX_NOT_SET");

        // a message coming from the counterpart gateway was executed by the bridge
        IBridge bridge = IInbox(inbox).bridge();
        require(msg.sender == address(bridge), "NOT_FROM_BRIDGE");

        // and the outbox reports that the L2 address of the sender is the counterpart gateway
        address l2ToL1Sender = IOutbox(bridge.activeOutbox()).l2ToL1Sender();
        require(l2ToL1Sender == l2Counterpart, "ONLY_COUNTERPART_GATEWAY");
        _;
    }
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L74-L77

## 10. Constant expressions are left as expressions, not constants.
This results in the keccak operation being performed whenever the variable is used, increasing gas costs relative to just storing the output hash.
*There are 4 instances of this issue:* 
```
bytes32 private constant DOMAIN_TYPE_HASH =
        keccak256(
            "EIP712Domain(string name,string version,uint256 chainId,address verifyingContract,bytes32 salt)"
        );
    bytes32 private constant DOMAIN_NAME_HASH = keccak256("Graph Token");
    bytes32 private constant DOMAIN_VERSION_HASH = keccak256("0");
  ...
    bytes32 private constant PERMIT_TYPEHASH =
        keccak256(
            "Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)"
        );
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L34-L45

## 11. Optimize names to save gas
public/external function names and public member variable names can be optimized to save gas. See [this] (https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92) link for an example of how it works. Below is the interface that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup.
```
interface IgraphToken
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/token/IGraphToken.sol#L7

## 12. Using bools for storage incurs overhead
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.
https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27
Use uint256(1) and uint256(2) for true/false to avoid for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from 'false' to 'true', after having been 'true' in the past.
*There are 3 instances of this issue:* 
```
bool internal _partialPaused;
    
    bool internal _paused;
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L8-L10
```
mapping(address => bool) private _minters;
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L51