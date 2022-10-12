## FINDINGS
NB: *Some functions have been truncated where neccessary to just show affected parts of the code*


### Cache storage values in memory to minimize SLOADs
The code can be optimized by minimizing the number of SLOADs.

SLOADs are expensive (100 gas after the 1st one) compared to MLOADs/MSTOREs (3 gas each). Storage values read multiple times should instead be cached in memory the first time (costing 1 SLOAD) and then read from this cache to avoid multiple SLOADs.

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L73-L84
### L1GraphTokenGateway.sol.onlyL2Counterpart(): inbox should be cached(Saves  1 SLOAD)
```solidity
File: /contracts/gateway/L1GraphTokenGateway.sol
73:    modifier onlyL2Counterpart() {
74:        require(inbox != address(0), "INBOX_NOT_SET"); //@audit: SLOAD 1

76:        // a message coming from the counterpart gateway was executed by the bridge
77:        IBridge bridge = IInbox(inbox).bridge(); //@audit: SLOAD 2
78:        require(msg.sender == address(bridge), "NOT_FROM_BRIDGE");

84:    }
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L263-L279
### L1GraphTokenGateway.sol.finalizeInboundTransfer(): escrow should be cached(Saves 1 SLOAD)
```solidity
File: /contracts/gateway/L1GraphTokenGateway.sol
263:    function finalizeInboundTransfer(
 
273:        uint256 escrowBalance = token.balanceOf(escrow); //@audit: SLOAD 1
274:        // If the bridge doesn't have enough tokens, something's very wrong!
275:        require(_amount <= escrowBalance, "BRIDGE_OUT_OF_FUNDS");
276:        token.transferFrom(escrow, _to, _amount); //@audit: SLOAD 2


279:    }
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L137-L175
### L2GraphTokenGateway.sol.outboundTransfer(): l1GRT should be cached(Saves 1 SLOAD)
```solidity
File: /contracts/l2/gateway/L2GraphTokenGateway.sol
137:    function outboundTransfer(

144:    ) public payable override notPaused nonReentrant returns (bytes memory) {
145:        require(_l1Token == l1GRT, "TOKEN_NOT_GRT"); //@audit: SLOAD 1

         
156: L2GraphToken(calculateL2TokenAddress(l1GRT)).bridgeBurn(outboundCalldata.from, _amount);//@audit: SLOAD 2

175:    }
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L226-L248
### L2GraphTokenGateway.sol.finalizeInboundTransfer(): l1GRT should be cached(Saves 1 SLOAD)
```solidity
File: /contracts/l2/gateway/L2GraphTokenGateway.sol
226:    function finalizeInboundTransfer(
 
232:    ) external payable override notPaused onlyL1Counterpart nonReentrant {
233:        require(_l1Token == l1GRT, "TOKEN_NOT_GRT"); //@audit: SLOAD 1
 
236:        L2GraphToken(calculateL2TokenAddress(l1GRT)).bridgeMint(_to, _amount);//@audit: SLOAD 2

248:    }
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L43-L46
### Managed.sol.\_notPartialPaused(): controller should be cached(Saves 1 SLOAD)
```solidity
File: /contracts/governance/Managed.sol
43:    function _notPartialPaused() internal view {
44:        require(!controller.paused(), "Paused"); //@audit: SLOAD 1
45:        require(!controller.partialPaused(), "Partial-paused");//@audit: SLOAD 2
46:    }
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L53-L67
### Governed.sol.acceptOwnership(): pendingGovernor should be cached(Saves ~3 SLOADs)
```solidity
File: /contracts/governance/Governed.sol
53:    function acceptOwnership() external {
54:        require(
55:            pendingGovernor != address(0) && msg.sender == pendingGovernor,
56:            "Caller must be pending governor"
57:        ); //@audit: SLOAD 1 and SLOAD 2

59:        address oldGovernor = governor;
60:        address oldPendingGovernor = pendingGovernor; //@audit: SLOAD 3

62:        governor = pendingGovernor; //@audit: SLOAD 4

67:    }
```

### Multiple accesses of a mapping/array should use a local variable cache

Caching a mapping's value in a local storage or calldata variable when the value is accessed multiple times saves ~42 gas per access due to not having to perform the same offset calculation every time.
**Help the Optimizer by saving a storage variable's reference instead of repeatedly fetching it**

To help the optimizer,declare a storage type variable and use it instead of repeatedly fetching the reference in a map or an array. 
As an example, instead of repeatedly calling ```someMap[someIndex]```, save its reference like this: ```SomeStruct storage someStruct = someMap[someIndex]``` and use it

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L152-L157
### L1GraphTokenGateway.sol.addToCallhookWhitelist(): callhookWhitelist[\_newWhitelisted] should be cached
```solidity
File: /contracts/gateway/L1GraphTokenGateway.sol
152:    function addToCallhookWhitelist(address _newWhitelisted) external onlyGovernor {
153:        require(_newWhitelisted != address(0), "INVALID_ADDRESS");
154:        require(!callhookWhitelist[_newWhitelisted], "ALREADY_WHITELISTED");
155:        callhookWhitelist[_newWhitelisted] = true;
156:        emit AddedToCallhookWhitelist(_newWhitelisted);
157:    }
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L164-L169
### L1GraphTokenGateway.sol.removeFromCallhookWhitelist(): callhookWhitelist[\_newWhitelisted] should be cached
```solidity
File: /contracts/gateway/L1GraphTokenGateway.sol
164:    function removeFromCallhookWhitelist(address _notWhitelisted) external onlyGovernor {
165:        require(_notWhitelisted != address(0), "INVALID_ADDRESS");
166:        require(callhookWhitelist[_notWhitelisted], "NOT_WHITELISTED");
167:        callhookWhitelist[_notWhitelisted] = false;
168:        emit RemovedFromCallhookWhitelist(_notWhitelisted);
169:    }
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L74-L99
### GraphTokenUpgradeable.sol.permit(): nonces[\_owner] should be cached
```solidity
File: /contracts/l2/token/GraphTokenUpgradeable.sol
74:    function permit(
 
87:                keccak256(
88:                    abi.encode(PERMIT_TYPEHASH, _owner, _spender, _value, nonces[_owner], _deadline) //@audit: nonces[_owner]

97:        nonces[_owner] = nonces[_owner].add(1); //@audit: nonces[_owner]
98:        _approve(_owner, _spender, _value);
99:    }

```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L173-L180
### Managed.sol.\_syncContract(): addressCache[nameHash] should be cached
```solidity
File: /contracts/governance/Managed.sol
173:    function _syncContract(string memory _name) internal {

176:        if (addressCache[nameHash] != contractAddress) {
177:            addressCache[nameHash] = contractAddress;
178:            emit ContractSynced(nameHash, contractAddress);
179:        }
180:    }
```

### Emitting storage values instead of the memory one.
Here, the values emitted shouldn’t be read from storage. The existing memory values should be used instead:

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L59-L63
```solidity
File: /contracts/l2/token/L2GraphToken.sol

//@audit: We should emit _gw instead of gateway
59:    function setGateway(address _gw) external onlyGovernor {
60:        require(_gw != address(0), "INVALID_GATEWAY");
61:        gateway = _gw;
62:        emit GatewaySet(gateway);
63:    }
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L26-L35
```solidity
File: /contracts/governance/Pausable.sol
26:    function _setPartialPaused(bool _toPause) internal {
27:        if (_toPause == _partialPaused) {
28:            return;
29:        }
30:        _partialPaused = _toPause;
31:        if (_partialPaused) {
32:            lastPausePartialTime = block.timestamp;
33:        }
34:        emit PartialPauseChanged(_partialPaused);
35:    }
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L40-L49
```solidity
File: /contracts/governance/Pausable.sol

//@audit: We should emit _toPause instead of _paused
40:    function _setPaused(bool _toPause) internal {
41:        if (_toPause == _paused) {
42:            return;
43:        }
44:        _paused = _toPause;
45:        if (_paused) {
46:            lastPauseTime = block.timestamp;
47:        }
48:        emit PauseChanged(_paused);
49:    }
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L55-L59
```solidity
File: /contracts/governance/Pausable.sol

//@audit: We should emit newPauseGuardian isntead of pauseGuardian
55:    function _setPauseGuardian(address newPauseGuardian) internal {
56:        address oldPauseGuardian = pauseGuardian;
57:        pauseGuardian = newPauseGuardian;
58:        emit NewPauseGuardian(oldPauseGuardian, pauseGuardian);
59:    }
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L40-L47
```solidity
File: /contracts/governance/Governed.sol

//@audit: We should emit _newGovernor instead of pendingGovernor
40:    function transferOwnership(address _newGovernor) external onlyGovernor {
      
44:        pendingGovernor = _newGovernor;

46:        emit NewPendingOwnership(oldPendingGovernor, pendingGovernor);
47:    }
```

### Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.


https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L51-L52
```solidity
File: /contracts/l2/token/GraphTokenUpgradeable.sol
51:    mapping(address => bool) private _minters;
52:    mapping(address => uint256) public nonces;
```

### Avoid contract existence checks by using solidity version 0.8.10 or later

Prior to 0.8.10 the compiler inserted extra code, including EXTCODESIZE (100 gas), to check for contract existence for external calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L77
```solidity
File: /contracts/gateway/L1GraphTokenGateway.sol
77:        IBridge bridge = IInbox(inbox).bridge();

81:        address l2ToL1Sender = IOutbox(bridge.activeOutbox()).l2ToL1Sender();


273:        uint256 escrowBalance = token.balanceOf(escrow);
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L44-L45
```solidity
File: /contracts/governance/Managed.sol
44:        require(!controller.paused(), "Paused");
45:        require(!controller.partialPaused(), "Partial-paused");

49:        require(!controller.paused(), "Paused");

53:        require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");


164:            contractAddress = controller.getContractProxy(_nameHash);

175:        address contractAddress = controller.getContractProxy(nameHash);
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L24
```solidity
File: /contracts/upgrades/GraphUpgradeable.sol
24:        require(msg.sender == _proxy.admin(), "Caller must be the proxy admin");
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/GraphTokenGateway.sol#L20
```solidity
File: /contracts/gateway/GraphTokenGateway.sol
20:            msg.sender == controller.getGovernor() || msg.sender == pauseGuardian,
```

### Expressions for constant values such as a call to keccak256(), should use immutable rather than constant

Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a constant variable is recomputed each time that the variable is used, which wastes some gas.

If the variable was immutable instead: the calculation would only be done once at deploy time (in the constructor), and then the result would be saved and read directly at runtime rather than being recalculated.


**consequences:**
-  Each usage of a "constant" costs ~100gas more on each access (it is still a little better than storing the result in storage, but not much..)

-  Since these are not real constants, they can't be referenced from a real constant environment (e.g. from assembly, or from another library )
 
See: ethereum/solidity#9232

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L34-L39
```solidity
File: /contracts/l2/token/GraphTokenUpgradeable.sol
34:    bytes32 private constant DOMAIN_TYPE_HASH =
35:        keccak256(
36:            "EIP712Domain(string name,string version,uint256 chainId,address verifyingContract,bytes32 salt)"
37:        );
38:    bytes32 private constant DOMAIN_NAME_HASH = keccak256("Graph Token");
39:    bytes32 private constant DOMAIN_VERSION_HASH = keccak256("0");

42:    bytes32 private constant PERMIT_TYPEHASH =
43:        keccak256(
44:            "Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)"
45:        );

```
**Proof: can be tested on remix also, showing results for foundry** 
```solidity
pragma solidity ^0.8.6;
contract Constant{
    bytes32 constant noteDenom = keccak256(bytes("NOTE"));
    function doConstant() external view returns(bytes32){
        return noteDenom;
    }
}
contract Immutable{
    bytes32 immutable noteDenom = keccak256(bytes("NOTE"));
    function doImmutable() external view returns(bytes32){
        return noteDenom;
    }
}
```

**Results from :** `forge test --gas-report --optimize`

```solidity
Running 1 test for test/Constant.t.sol:ConstantTest
[PASS] testdoThing() (gas: 5416)
Test result: ok. 1 passed; 0 failed; finished in 420.08µs

Running 1 test for test/Immutable.t.sol:ImmutableTest
[PASS] testdoThing() (gas: 5356)
Test result: ok. 1 passed; 0 failed; finished in 389.91µs
```

### Splitting require() statements that use && saves gas - (saves 8 gas per &&)

Instead of using the && operator in a single require statement to check multiple conditions,using multiple require statements with 1 condition per require statement will save 8 GAS per &&
The gas difference would only be realized if the revert condition is realized(met).

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L142
```solidity
File: /contracts/gateway/L1GraphTokenGateway.sol
142:        require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L142-L145
```solidity
File: /contracts/upgrades/GraphProxy.sol
142:        require(
143:            _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
144:            "Caller must be the pending implementation"
145:        );
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L54-L57
```solidity
File: /contracts/governance/Governed.sol
54:        require(
55:            pendingGovernor != address(0) && msg.sender == pendingGovernor,
56:            "Caller must be pending governor"
57:        );
```
**Proof**
**The following tests were carried out in remix with both optimization turned on and off**

```solidity
function multiple (uint a) public pure returns (uint){
	require ( a > 1 && a < 5, "Initialized");
	return  a + 2;
}
```
**Execution cost**
21617 with optimization and using &&
21976 without optimization and using &&

After splitting the require statement
```solidity
function multiple(uint a) public pure returns (uint){
	require (a > 1 ,"Initialized");
	require (a < 5 , "Initialized");
	return a + 2;
}
```
**Execution cost**
21609 with optimization and split require
21968 without optimization and using split require


### Pre-Solidity 0.8.13: > 0 is less efficient than != 0 for unsigned integers(6 gas)

Up until Solidity 0.8.13: != 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas)
!= 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas)

For uints the minimum value would be 0 and never a negative value. Since it cannot be a negative value, then the check > 0 is essentially checking that the value is not equal to 0 therefore >0 can be replaced with !=0 which saves gas.

Proof: While it may seem that > 0 is cheaper than !=, this is only true without the optimizer enabled and outside a require statement. If you enable the optimizer at 10k AND you're in a require statement, this will save gas. You can see this tweet for more proofs: https://twitter.com/gzeon/status/1485428085885640706

I suggest changing > 0 with != 0 here:

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L201
```solidity
File: /contracts/gateway/L1GraphTokenGateway.sol
201:        require(_amount > 0, "INVALID_ZERO_AMOUNT");

217:                require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L146
```solidity
File: /contracts/l2/gateway/L2GraphTokenGateway.sol
146:        require(_amount > 0, "INVALID_ZERO_AMOUNT");
```

### Boolean comparisons

Comparing to a constant (true or false) is a bit more expensive than directly checking the returned boolean value.
I suggest using if(directValue) instead of if(directValue == true) and if(!directValue) instead of if(directValue == false) here:

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L213-L216
```solidity
File: /contracts/gateway/L1GraphTokenGateway.sol

//@audit: callhookWhitelist[msg.sender] == true,
213:                require(
214:                    extraData.length == 0 || callhookWhitelist[msg.sender] == true,
215:                    "CALL_HOOK_DATA_NOT_ALLOWED"
216:                );
```
### Using bools for storage incurs overhead
```
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.
```

See [source](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27) 
Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past

**Instances affected include**

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L35
```solidity
File: /contracts/gateway/L1GraphTokenGateway.sol
35:    mapping(address => bool) public callhookWhitelist;
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L51
```solidity
File: /contracts/l2/token/GraphTokenUpgradeable.sol
51:    mapping(address => bool) private _minters;
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L8-L10
```solidity
File: /contracts/governance/Pausable.sol
8:    bool internal _partialPaused;
9:    // Paused will pause all major protocol functions
10:    bool internal _paused;
```
### A modifier used only once and not being inherited should be inlined to save gas
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L73-L84
```solidity
File: /contracts/gateway/L1GraphTokenGateway.sol
73:    modifier onlyL2Counterpart() {
74:        require(inbox != address(0), "INBOX_NOT_SET");

76:        // a message coming from the counterpart gateway was executed by the bridge
77:        IBridge bridge = IInbox(inbox).bridge();
78:        require(msg.sender == address(bridge), "NOT_FROM_BRIDGE");


80:        // and the outbox reports that the L2 address of the sender is the counterpart gateway
81:        address l2ToL1Sender = IOutbox(bridge.activeOutbox()).l2ToL1Sender();
82:        require(l2ToL1Sender == l2Counterpart, "ONLY_COUNTERPART_GATEWAY");
83:        _;
84:    }
```

The above modifier is only used once on [Line 269](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L269)


### abi.encode() is less efficient than abi.encodePacked()
Changing abi.encode function to abi.encodePacked can save gas since the abi.encode function pads extra null bytes at the end of the call data, which is unnecessary. 
Consider using abi.encodePacked() here:

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L249
```solidity
File: /contracts/gateway/L1GraphTokenGateway.sol
249:        return abi.encode(seqNum);
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L174
```solidity
File: /contracts/l2/gateway/L2GraphTokenGateway.sol
174:        return abi.encode(id);
```
### Use shorter revert strings(less than 32 bytes) 
Every reason string takes at least 32 bytes so make sure your string fits in 32 bytes or it will become more expensive.Each extra chunk of byetes past the original 32 incurs an MSTORE which costs 3 gas

Shortening revert strings to fit in 32 bytes will decrease deployment time gas and will decrease runtime gas when the revert condition is met.
Revert strings that are longer than 32 bytes require at least one additional mstore, along with additional overhead for computing memory offset, etc.

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L105
```solidity
File: /contracts/upgrades/GraphProxy.sol
105:        require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");

141:        require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
142:        require(
143:            _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
144:            "Caller must be the pending implementation"
145:        );
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L53
```solidity
File: /contracts/governance/Managed.sol
53:        require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/GraphTokenGateway.sol#L19-L22
```solidity
File: /contracts/gateway/GraphTokenGateway.sol
19:        require(
20:            msg.sender == controller.getGovernor() || msg.sender == pauseGuardian,
21:            "Only Governor or Guardian can call"
22:        );
```
I suggest shortening the revert strings to fit in 32 bytes, or using custom errors.


### Use a more recent version of solidity

Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

Other than the interfaces, the rest of the code seems to be using the following version.

```solidity
   pragma solidity ^0.7.6;
```

An example of how where we might utilise the custom errors is explained below

### Use Custom Errors instead of Revert Strings to save Gas(~50 gas)
Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met)
Custom errors save ~50 gas each time they’re hit by avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

Custom errors are defined using the error statement, which can be used inside and outside of contracts (including interfaces and libraries).
see [Source](https://blog.soliditylang.org/2021/04/21/custom-errors/)

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L24
```solidity
File: /contracts/upgrades/GraphUpgradeable.sol
24:        require(msg.sender == _proxy.admin(), "Caller must be the proxy admin");

32:        require(msg.sender == _implementation(), "Caller must be the implementation");
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L24
```solidity
File: /contracts/governance/Governed.sol
24:        require(msg.sender == governor, "Only Governor can call");

41:        require(_newGovernor != address(0), "Governor must be set");

54:        require(
55:            pendingGovernor != address(0) && msg.sender == pendingGovernor,
56:            "Caller must be pending governor"
57:        );
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L36
```solidity
File: /contracts/l2/token/L2GraphToken.sol
36:        require(msg.sender == gateway, "NOT_GATEWAY");

49:        require(_owner != address(0), "Owner must be set");

60:        require(_gw != address(0), "INVALID_GATEWAY");

70:        require(_addr != address(0), "INVALID_L1_ADDRESS");
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyStorage.sol#L62
```solidity
File: /contracts/upgrades/GraphProxyStorage.sol
62:        require(msg.sender == _admin(), "Caller must be admin");
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L105
```solidity
File: /contracts/upgrades/GraphProxy.sol
105:        require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");

141:        require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
142:        require(
143:            _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
144:            "Caller must be the pending implementation"
145:        );


157:        require(msg.sender != _admin(), "Cannot fallback to proxy target");
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L44-L45
```solidity
File: /contracts/governance/Managed.sol
44:        require(!controller.paused(), "Paused");
45:        require(!controller.partialPaused(), "Partial-paused");

49:        require(!controller.paused(), "Paused");

53:        require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");

57:        require(msg.sender == address(controller), "Caller must be Controller");

104:        require(_controller != address(0), "Controller must be set");


```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/GraphTokenGateway.sol#L19-L22
```solidity
File: /contracts/gateway/GraphTokenGateway.sol
19:        require(
20:            msg.sender == controller.getGovernor() || msg.sender == pauseGuardian,
21:            "Only Governor or Guardian can call"
22:        );

31:        require(_newPauseGuardian != address(0), "PauseGuardian must be set");

40:        require(!_paused, "Paused (contract)");


```
