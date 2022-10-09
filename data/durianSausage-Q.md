## low risks
### L01: the assert() function when false, uses up all the remaining gas and reverts all the changes made.
#### problem
assert(false) compiles to 0xfe, which is an invalid opcode, using up all remaining gas, and reverting all changes.
require(false) compiles to 0xfd which is the REVERT opcode, meaning it will refund the remaining gas. The opcode can also return a value (useful for debugging), but I don't believe that is supported in Solidity as of this moment. (2017-11-21)
#### prof
upgrades/GraphProxy.sol, 47, b'        assert(ADMIN_SLOT == bytes32(uint256(keccak256("eip1967.proxy.admin")) - 1));'
upgrades/GraphProxy.sol, 48, b'        assert('
upgrades/GraphProxy.sol, 51, b'        assert('


### L02: INPUT ARRAY LENGTHS MAY DIFFER
#### problem
If the caller makes a copy-paste error, the lengths may be mismatchd and an operation believed to have been completed may not in fact have been completed
function withdrawMultipleERC721(address[] memory _tokens, uint256[] memory _tokenId, address _to) external override {
#### prof
rewards/IRewardsManager.sol, 29, b'    function setDeniedMany(bytes32[] calldata _subgraphDeploymentID, bool[] calldata _deny)\n        external;'

### L03: address variable should check if it is zero MISSING CHECKS FOR ADDRESS(0X0) WHEN ASSIGNING VALUES TO ADDRESS STATE VARIABLES
#### prof
governance/Governed.sol, 32, b'        governor = _initGovernor;'
governance/Governed.sol, 44, b'        pendingGovernor = _newGovernor;'
governance/Governed.sol, 62, b'        governor = pendingGovernor;'
governance/Governed.sol, 63, b'        pendingGovernor = address(0);'
governance/Pausable.sol, 57, b'        pauseGuardian = newPauseGuardian;'