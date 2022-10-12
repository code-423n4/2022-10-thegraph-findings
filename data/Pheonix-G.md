|  | Issue | Instances |
| --- | --- | --- |
| [G-01] | Use Custom Errors instead of Revert Strings to save Gas | 57 |
| [G-03] | Constant expressions | 5 |
| [G-03] | Using bool for storage incurs overhead | 2 |
| [G-04] | abi.encode() is less efficient than abi.encodePacked() | 4 |
| [G-05] |  != is more efficient than > in require | 3 |
| [G-06] |  Splitting require() statements that use && | 4 |

# [G-01] **Use Custom Errors instead of Revert Strings**

1. Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met) Custom errors save ~50 gas each time they’re hit by avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

Custom errors are defined using the error statement, which can be used inside and outside of contracts (including interfaces and libraries). see [Source](https://blog.soliditylang.org/2021/04/21/custom-errors/)

## **INSTANCES**

### File: Governed.sol

24:         require(msg.sender == governor, "Only Governor can call"); 

41: require(_newGovernor != address(0), "Governor must be set"); 

54:         require(
55:             pendingGovernor != address(0) && msg.sender == pendingGovernor,
56:             "Caller must be pending governor"
57:         );

### File: GraphProxy.sol

105:         require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");

141:         require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
142:         require(
143:             _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
144:             "Caller must be the pending implementation"
145:         );

157: require(msg.sender != _admin(), "Cannot fallback to proxy target");

### File:GraphProxyStorage.sol

62:         require(msg.sender == _admin(), "Caller must be admin");

### File:GraphTokenGateway.sol

19:         require(
20:             msg.sender == controller.getGovernor() || msg.sender == pauseGuardian,
21:             "Only Governor or Guardian can call"
22:         );

31: require(_newPauseGuardian != address(0), "PauseGuardian must be set");
40: require(!_paused, "Paused (contract)");

### File:GraphTokenUpgradeable.sol

60:         require(isMinter(msg.sender), "Only minter can call"); 

94:         require(_owner == recoveredAddress, "GRT: invalid permit"); 
95:         require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");

106:         require(_account != address(0), "INVALID_MINTER");

115: require(_minters[_account], "NOT_A_MINTER");

123: require(_minters[msg.sender], "NOT_A_MINTER");

122: require(_l2GRT != address(0), "INVALID_L2_GRT");  

132: require(_l2Counterpart != address(0), "INVALID_L2_COUNTERPART");

### File:GraphUpgradeable.sol

24:         require(msg.sender == _proxy.admin(), "Caller must be the proxy admin"); 

32: require(msg.sender == _implementation(), "Caller must be the implementation");
82: require(l2ToL1Sender == l2Counterpart, "ONLY_COUNTERPART_GATEWAY"); // custom error

110:         require(_inbox != address(0), "INVALID_INBOX");// custom error
111:         require(_l1Router != address(0), "INVALID_L1_ROUTER");

### File:L1GraphTokenGateway.sol

74:         require(inbox != address(0), "INBOX_NOT_SET");

78: require(msg.sender == address(bridge), "NOT_FROM_BRIDGE");

142: require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW"); 

153:         require(_newWhitelisted != address(0), "INVALID_ADDRESS");
154:         require(!callhookWhitelist[_newWhitelisted], "ALREADY_WHITELISTED");

165:         require(_notWhitelisted != address(0), "INVALID_ADDRESS"); 
166:         require(callhookWhitelist[_notWhitelisted], "NOT_WHITELISTED");

200:         require(_l1Token == address(token), "TOKEN_NOT_GRT");
201:         require(_amount > 0, "INVALID_ZERO_AMOUNT");
202:         require(_to != address(0), "INVALID_DESTINATION");

213:                 require(
214:                     extraData.length == 0 || callhookWhitelist[msg.sender] == true,
215:                     "CALL_HOOK_DATA_NOT_ALLOWED"
216:                 );
217:                 require(maxSubmissionCost > 0, "NO_SUBMISSION_COST"); 

224:                     require(msg.value >= expectedEth, "WRONG_ETH_VALUE"); 
271: require(_l1Token == address(token), "TOKEN_NOT_GRT");

275: require(_amount <= escrowBalance, "BRIDGE_OUT_OF_FUNDS");

### File: L2GraphToken.sol

36:         require(msg.sender == gateway, "NOT_GATEWAY"); 

49: require(_owner != address(0), "Owner must be set");

60: require(_gw != address(0), "INVALID_GATEWAY");

70: require(_addr != address(0), "INVALID_L1_ADDRESS");

### File:L2GraphTokenGateway.sol

69:         require( 
70:             msg.sender == AddressAliasHelper.applyL1ToL2Alias(l1Counterpart),
71:             "ONLY_COUNTERPART_GATEWAY"
72:         );
73:         _;

98: require(_l2Router != address(0), "INVALID_L2_ROUTER");

108: require(_l1GRT != address(0), "INVALID_L1_GRT");

118: require(_l1Counterpart != address(0), "INVALID_L1_COUNTERPART");

145:         require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
146:         require(_amount > 0, "INVALID_ZERO_AMOUNT");
147:         require(msg.value == 0, "INVALID_NONZERO_VALUE");
148:         require(_to != address(0), "INVALID_DESTINATION");
153: require(outboundCalldata.extraData.length == 0, "CALL_HOOK_DATA_NOT_ALLOWED");

233:         require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
234:         require(msg.value == 0, "INVALID_NONZERO_VALUE");

### File: Managed.sol

44:         require(!controller.paused(), "Paused");
45:         require(!controller.partialPaused(), "Partial-paused");
46:     }
47:
48:     function _notPaused() internal view virtual {
49:         require(!controller.paused(), "Paused"); 
50:     }
51:
52:     function _onlyGovernor() internal view {
53:         require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
54:     }
55:
56:     function _onlyController() internal view {
57:         require(msg.sender == address(controller), "Caller must be Controller"); 
58:     }

104: require(_controller != address(0), "Controller must be set"); 

### Recommended Mitigation Steps

Replace require and revert statements with custom errors.

# [G-02] **Constant expressions**

Constant expressions are [re-calculated each time they are in use](https://github.com/ethereum/solidity/issues/9232) , costing an extra `97`
 gas than a constant every time they are called.

## Instances -

### File: GraphTokenUpgradeable.sol

34:     bytes32 private constant DOMAIN_TYPE_HASH =
35:         keccak256(
36:             "EIP712Domain(string name,string version,uint256 chainId,address verifyingContract,bytes32 salt)"
37:         );
38:     bytes32 private constant DOMAIN_NAME_HASH = keccak256("Graph Token");
39:     bytes32 private constant DOMAIN_VERSION_HASH = keccak256("0");
40:     bytes32 private constant DOMAIN_SALT =
41:         0xe33842a7acd1d5a1d28f25a931703e5605152dc48d64dc4716efdae1f5659591; // Randomly generated salt
42:     bytes32 private constant PERMIT_TYPEHASH =
43:         keccak256(
44:             "Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)"
45:         );
46:

### Recommended Mitigation Steps

Mark these as `immutable` instead of `constant`.

# [G-03] **Using bool for storage incurs overhead**

Booleans are more expensive than uint256 or any type that 
takes up a full word because each write operation emits an extra SLOAD 
to first read the slot's contents, replace the bits taken up by the 
boolean, and then write back. This is the compiler's defense against 
contract upgrades and pointer aliasing, and it cannot be disabled.

Consider doing like here: [https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27) . They are using `uint256(1)` and `uint256(2)` for true/false to avoid the extra SLOAD (**100 gas**) and the extra SSTORE (**20000 gas**) when changing from `false` to `true`, after having been `true` in the past:

## **Instances -**

### File: GraphTokenUpgradeable.sol

51:     mapping(address => bool) private _minters; 

### File:L1GraphTokenGateway.sol

35:     mapping(address => bool) public callhookWhitelist; // BOOL TAKES MORE GAS THAN UINT

# [G-04] **abi.encode() is less efficient than abi.encodePacked()**

Changing `abi.encode` function to `abi.encodePacked`
 can save gas since the `abi.encode` function pads extra null bytes at the end of the call data, which is unnecessary. Also, in general, `abi.encodePacked` is more gas-efficient (see [Solidity-Encode-Gas-Comparison](https://github.com/ConnorBlockchain/Solidity-Encode-Gas-Comparison)).

### **Instances -**

### File: GraphTokenUpgradeable.sol

88:                     abi.encode(PERMIT_TYPEHASH, _owner, _spender, _value, nonces[_owner], _deadline)

162:             abi.encode(
163:                 DOMAIN_TYPE_HASH,
164:                 DOMAIN_NAME_HASH,
165:                 DOMAIN_VERSION_HASH,
166:                 _getChainID(),
167:                 address(this),
168:                 DOMAIN_SALT
169:             )

### File: L1GraphTokenGateway.sol

342:                 abi.encode(emptyBytes, _data)
343:             );

### File: L2GraphTokenGateway.sol

275:                 abi.encode(0, _data) // we don't need to track exitNums (b/c we have no fast exits) so we always use 0
276:             );

### Recommended Mitigation Steps

Change  `abi.encode` function to `abi.encodePacked`

# [G-05] != is more efficient than > in require

!= 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas)

For uints the minimum value would be 0 and never a negative value. Since it cannot be a negative value, then the check > 0 is essentially checking that the value is not equal to 0 therefore >0 can be replaced with !=0 which saves gas.

Proof: While it may seem that > 0 is cheaper than !=, this is only true without the optimizer enabled and outside a require statement. If you enable the optimizer at 10k AND you’re in a require statement, this will save gas. You can see this tweet for more proofs: [https://twitter.com/gzeon/status/1485428085885640706](https://twitter.com/gzeon/status/1485428085885640706)

## **Instances -**

### File: L1GraphTokenGateway.sol

201:         require(_amount > 0, "INVALID_ZERO_AMOUNT");

217: require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");

### File: L2GraphTokenGateway.sol

146:         require(_amount > 0, "INVALID_ZERO_AMOUNT");

### Recommended Mitigation Steps

changing > 0 with != 0 

# [G-06] **Splitting require() statements that use &&**

See [this issue](https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) which describes the fact that there is a larger deployment gas cost, 
but with enough runtime calls, the change ends up being cheaper by **3 gas**

## **Instances -**

### File: Governed.sol

54:         require(
55:             pendingGovernor != address(0) && msg.sender == pendingGovernor,
56:             "Caller must be pending governor"
57:         );

### File: GraphProxy.sol

142:         require(
143:             _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
144:             "Caller must be the pending implementation"
145:         );

### File:L1GraphTokenGateway.sol

142:         require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");

### File: Governed.sol

54:         require(
55:             pendingGovernor != address(0) && msg.sender == pendingGovernor,
56:             "Caller must be pending governor"
57:         );