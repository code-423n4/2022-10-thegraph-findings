1)Use custom errors rather than revert()/require() strings to save gas 
  
File: 2022-10-thegraph\contracts\governance\Managed.sol:
   43      function _notPartialPaused() internal view {
   44:         require(!controller.paused(), "Paused");
   45:         require(!controller.partialPaused(), "Partial-paused");
   46      }

   48      function _notPaused() internal view virtual {
   49:         require(!controller.paused(), "Paused");
   50      }

   52      function _onlyGovernor() internal view {
   53:         require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
   54      }

   56      function _onlyController() internal view {
   57:         require(msg.sender == address(controller), "Caller must be Controller");
   58      }

  103      function _setController(address _controller) internal {
  104:         require(_controller != address(0), "Controller must be set");
  105          controller = IController(_controller);

File: 2022-10-thegraph\contracts\l2\gateway\L2GraphTokenGateway.sol:
   68      modifier onlyL1Counterpart() {
   69:         require(
   70              msg.sender == AddressAliasHelper.applyL1ToL2Alias(l1Counterpart),

   97      function setL2Router(address _l2Router) external onlyGovernor {
   98:         require(_l2Router != address(0), "INVALID_L2_ROUTER");
   99          l2Router = _l2Router;

  107      function setL1TokenAddress(address _l1GRT) external onlyGovernor {
  108:         require(_l1GRT != address(0), "INVALID_L1_GRT");
  109          l1GRT = _l1GRT;

  117      function setL1CounterpartAddress(address _l1Counterpart) external onlyGovernor {
  118:         require(_l1Counterpart != address(0), "INVALID_L1_COUNTERPART");
  119          l1Counterpart = _l1Counterpart;

  144      ) public payable override notPaused nonReentrant returns (bytes memory) {
  145:         require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
  146:         require(_amount > 0, "INVALID_ZERO_AMOUNT");
  147:         require(msg.value == 0, "INVALID_NONZERO_VALUE");
  148:         require(_to != address(0), "INVALID_DESTINATION");
  149  

  152          (outboundCalldata.from, outboundCalldata.extraData) = parseOutboundData(_data);
  153:         require(outboundCalldata.extraData.length == 0, "CALL_HOOK_DATA_NOT_ALLOWED");
  154  

  232      ) external payable override notPaused onlyL1Counterpart nonReentrant {
  233:         require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
  234:         require(msg.value == 0, "INVALID_NONZERO_VALUE");
  235  

File: 2022-10-thegraph\contracts\l2\token\L2GraphToken.sol:
  35      modifier onlyGateway() {
  36:         require(msg.sender == gateway, "NOT_GATEWAY");
  37          _;

  48      function initialize(address _owner) external onlyImpl {
  49:         require(_owner != address(0), "Owner must be set");
  50          // Initial supply hard coded to 0 as tokens are only supposed

  59      function setGateway(address _gw) external onlyGovernor {
  60:         require(_gw != address(0), "INVALID_GATEWAY");
  61          gateway = _gw;

  69      function setL1Address(address _addr) external onlyGovernor {
  70:         require(_addr != address(0), "INVALID_L1_ADDRESS");
  71          l1Address = _addr;

File: 2022-10-thegraph\contracts\upgrades\GraphProxy.sol:
  104      function setAdmin(address _newAdmin) external ifAdmin {
  105:         require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
  106          _setAdmin(_newAdmin);

  132          (bool success, ) = _implementation().delegatecall(data);
  133:         require(success);
  134      }

  140          address _pendingImplementation = _pendingImplementation();
  141:         require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
  142:         require(
  143              _pendingImplementation != address(0) && msg.sender == _pendingImplementation,

  156      function _fallback() internal {
  157:         require(msg.sender != _admin(), "Cannot fallback to proxy target");
  158  

File: 2022-10-thegraph\contracts\upgrades\GraphProxyStorage.sol:
  61      modifier onlyAdmin() {
  62:         require(msg.sender == _admin(), "Caller must be admin");
  63          _;

File: 2022-10-thegraph\contracts\upgrades\GraphUpgradeable.sol:
  23      modifier onlyProxyAdmin(IGraphProxy _proxy) {
  24:         require(msg.sender == _proxy.admin(), "Caller must be the proxy admin");
  25          _;

  31      modifier onlyImpl() {
  32:         require(msg.sender == _implementation(), "Caller must be the implementation");
  33          _;  
  
2)USE A MORE RECENT VERSION OF SOLIDITY

Use a solidity version of at least 0.8.2 to get compiler automatic inlining

Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads

Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings

Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value
  

File: 2022-10-thegraph\contracts\curation\ICuration.sol:
  2  
  3: pragma solidity ^0.7.6;
  4  

File: 2022-10-thegraph\contracts\curation\IGraphCurationToken.sol:
  2  
  3: pragma solidity ^0.7.6;
  4  

File: 2022-10-thegraph\contracts\gateway\BridgeEscrow.sol:
  2  
  3: pragma solidity ^0.7.6;
  4  

File: 2022-10-thegraph\contracts\upgrades\GraphUpgradeable.sol:
  2  
  3: pragma solidity ^0.7.6;
  4  

File: 2022-10-thegraph\contracts\governance\Governed.sol:
  2  
  3: pragma solidity ^0.7.6;
  4 

File: 2022-10-thegraph\contracts\governance\Pausable.sol:
  2  
  3: pragma solidity ^0.7.6;
  4  


File: 2022-10-thegraph\contracts\l2\token\L2GraphToken.sol:
  2  
  3: pragma solidity ^0.7.6;
  4 

File: 2022-10-thegraph\contracts\upgrades\GraphProxyAdmin.sol:
  2  
  3: pragma solidity ^0.7.6;
  4  

File: 2022-10-thegraph\contracts\upgrades\GraphProxyStorage.sol:
  2  
  3: pragma solidity ^0.7.6;
  4  

File: 2022-10-thegraph\contracts\upgrades\GraphProxy.sol:
  2  
  3: pragma solidity ^0.7.6;
  4  

File: 2022-10-thegraph\contracts\governance\Managed.sol:
  2  
  3: pragma solidity ^0.7.6;
  4  

File: 2022-10-thegraph\contracts\l2\token\GraphTokenUpgradeable.sol:
  2  
  3: pragma solidity ^0.7.6;
  4 

File: 2022-10-thegraph\contracts\l2\gateway\L2GraphTokenGateway.sol:
  2  
  3: pragma solidity ^0.7.6;
  4  pragma abicoder v2;

File: 2022-10-thegraph\contracts\gateway\L1GraphTokenGateway.sol:
  2  
  3: pragma solidity ^0.7.6;
  4  pragma abicoder v2;  
  
File: 2022-10-thegraph\contracts\gateway\GraphTokenGateway.sol:
  2  
  3: pragma solidity ^0.7.6;
  4 

File: 2022-10-thegraph\contracts\curation\IGraphCurationToken.sol:
  2  
  3: pragma solidity ^0.7.6;
  4  

File: 2022-10-thegraph\contracts\gateway\ICallhookReceiver.sol:
  8   */
  9: pragma solidity ^0.7.6;
  10 

File: 2022-10-thegraph\contracts\upgrades\IGraphProxy.sol:
  2  
  3: pragma solidity ^0.7.6;
  4 

File: 2022-10-thegraph\contracts\epochs\IEpochManager.sol:
  2  
  3: pragma solidity ^0.7.6;
  4  

File: 2022-10-thegraph\contracts\governance\IController.sol:
  2  
  3: pragma solidity >=0.6.12 <0.8.0;
  4  

File: 2022-10-thegraph\contracts\token\IGraphToken.sol:
  2  
  3: pragma solidity ^0.7.6;
  4 

File: 2022-10-thegraph\contracts\rewards\IRewardsManager.sol:
  2  
  3: pragma solidity ^0.7.6;
  4  

File: 2022-10-thegraph\contracts\staking\IStakingData.sol:
  2  
  3: pragma solidity >=0.6.12 <0.8.0;
  4  

File: 2022-10-thegraph\contracts\curation\ICuration.sol:
  2  
  3: pragma solidity ^0.7.6;
  4  

File: 2022-10-thegraph\contracts\staking\IStaking.sol:
  2  
  3: pragma solidity >=0.6.12 <0.8.0;
  4  pragma abicoder v2;

3)STATE VARIABLES CAN BE PACKED INTO FEWER STORAGE SLOTS
If variables occupying the same slot are both written the same function or by the constructor,
avoids a separate Gsset (20000 gas). Reads of the variables are also cheaper

File: 2022-10-thegraph\contracts\token\IGraphToken.sol

  
29      address _owner,
30      address _spender,
31      uint256 _value,
32      uint256 _deadline,
33      uint8 _v,
34      bytes32 _r,
35      bytes32 _s  

  
		
 Variable ordering with 6 slots instead of the current 7:

   uint8(1):_v, address(20):_owner, address(20):_spender, uint256(32):_value, uint256(32):_deadline
   bytes32(32):_r, bytes32(32):_s    
   
   
4)Splitting require() statements that use && saves gas
   
   Instead of using the && operator in a single require statement to check multiple
conditions,using multiple require statements with 1 condition per require statement
will save 3 GAS per && :
// Before
require(result >= MIN_64x64 && result <= MAX_64x64);
// After
require(result >= MIN_64x64);
require(result <= MAX_64x64);


File: 2022-10-thegraph\contracts\gateway\L1GraphTokenGateway.sol:
  
  142:         require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
  
  
File: 2022-10-thegraph\contracts\governance\Governed.sol
  55,43:             pendingGovernor != address(0) && msg.sender == pendingGovernor,
                     "Caller must be pending governor"
                     );
					 
File: 2022-10-thegraph\contracts\staking\Staking.sol
  369,37:         require(_alphaNumerator > 0 && _alphaDenominator > 0, "!alpha");
  
 
File: 2022-10-thegraph\contracts\upgrades\GraphProxy.sol
  142,48:           require(
  143,50:             _pendingImplementation != address(0) && msg.sender == _pendingImplementation, 
                      "Caller must be the pending implementation"
                     );  
					 
  
5) USING > 0 COSTS MORE GAS THAN != 0 WHEN USED ON A UINT IN A REQUIRE() STATEMENT

This change saves 6 gas per instance

File: 2022-10-thegraph\contracts\gateway\L1GraphTokenGateway.sol:
  200          require(_l1Token == address(token), "TOKEN_NOT_GRT");
  201:         require(_amount > 0, "INVALID_ZERO_AMOUNT");
  202          require(_to != address(0), "INVALID_DESTINATION");

  216                  );
  217:                 require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");  
  
  
File: 2022-10-thegraph\contracts\l2\gateway\L2GraphTokenGateway.sol:

  146:         require(_amount > 0, "INVALID_ZERO_AMOUNT");
   
 
 