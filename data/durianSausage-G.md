### G01: custom error save more gas
#### problem
Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met) while providing the same amount of information, as explained https://blog.soliditylang.org/2021/04/21/custom-errors/. 
Custom errors are defined using the error statement.
#### prof
governance/Governed.sol, 41, b'        require(_newGovernor != address(0), "Governor must be set");'
governance/Governed.sol, 57, b'        require(\n            pendingGovernor != address(0) && msg.sender == pendingGovernor,\n            "Caller must be pending governor"\n        );'
upgrades/GraphProxy.sol, 105, b'        require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");'
upgrades/GraphProxy.sol, 141, b'        require(Address.isContract(_pendingImplementation), "Implementation must be a contract");'
upgrades/GraphProxy.sol, 145, b'        require(\n            _pendingImplementation != address(0) && msg.sender == _pendingImplementation,\n            "Caller must be the pending implementation"\n        );'
upgrades/GraphProxy.sol, 157, b'        require(msg.sender != _admin(), "Cannot fallback to proxy target");'

### G02: Splitting require() statements that use && saves gas
#### problem
See this issue(https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper
#### prof:
governance/Governed.sol, 57, b'        require(\n            pendingGovernor != address(0) && msg.sender == pendingGovernor,\n            "Caller must be pending governor"\n        );'
upgrades/GraphProxy.sol, 145, b'        require(\n            _pendingImplementation != address(0) && msg.sender == _pendingImplementation,\n            "Caller must be the pending implementation"\n        );'

### G03: FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE
#### problem
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost
#### prof
governance/Governed.sol, 47, b'    function transferOwnership(address _newGovernor) external onlyGovernor '
governance/Governed.sol, 47, b'    function transferOwnership(address _newGovernor) external onlyGovernor '
governance/Governed.sol, 67, b'    function acceptOwnership() external '
governance/Governed.sol, 67, b'    function acceptOwnership() external '
upgrades/GraphProxyAdmin.sol, 61, b'    function getProxyAdmin(IGraphProxy _proxy) public view returns (address) '
upgrades/GraphProxyAdmin.sol, 61, b'    function getProxyAdmin(IGraphProxy _proxy) public view returns (address) '
upgrades/GraphProxyAdmin.sol, 70, b'    function changeProxyAdmin(IGraphProxy _proxy, address _newAdmin) public onlyGovernor '
upgrades/GraphProxyAdmin.sol, 70, b'    function changeProxyAdmin(IGraphProxy _proxy, address _newAdmin) public onlyGovernor '
upgrades/GraphProxy.sol, 71, b'    function admin() external ifAdminOrPendingImpl returns (address) '
upgrades/GraphProxy.sol, 71, b'    function admin() external ifAdminOrPendingImpl returns (address) '
upgrades/GraphProxy.sol, 84, b'    function implementation() external ifAdminOrPendingImpl returns (address) '
upgrades/GraphProxy.sol, 84, b'    function implementation() external ifAdminOrPendingImpl returns (address) '
upgrades/GraphProxy.sol, 97, b'    function pendingImplementation() external ifAdminOrPendingImpl returns (address) '
upgrades/GraphProxy.sol, 97, b'    function pendingImplementation() external ifAdminOrPendingImpl returns (address) '
upgrades/GraphProxy.sol, 107, b'    function setAdmin(address _newAdmin) external ifAdmin '
upgrades/GraphProxy.sol, 107, b'    function setAdmin(address _newAdmin) external ifAdmin '
upgrades/GraphProxy.sol, 117, b'    function upgradeTo(address _newImplementation) external ifAdmin '
upgrades/GraphProxy.sol, 117, b'    function upgradeTo(address _newImplementation) external ifAdmin '
upgrades/GraphProxy.sol, 124, b'    function acceptUpgrade() external ifAdminOrPendingImpl '
upgrades/GraphProxy.sol, 124, b'    function acceptUpgrade() external ifAdminOrPendingImpl '
upgrades/GraphProxy.sol, 134, b'    function acceptUpgradeAndCall(bytes calldata data) external ifAdminOrPendingImpl '
upgrades/GraphProxy.sol, 134, b'    function acceptUpgradeAndCall(bytes calldata data) external ifAdminOrPendingImpl '
upgrades/GraphProxy.sol, 185, b'    function _fallback() internal '
upgrades/GraphUpgradeable.sol, 52, b'    function acceptProxy(IGraphProxy _proxy) external onlyProxyAdmin(_proxy) '
upgrades/GraphUpgradeable.sol, 52, b'    function acceptProxy(IGraphProxy _proxy) external onlyProxyAdmin(_proxy) '
upgrades/GraphUpgradeable.sol, 64, b'    function acceptProxyAndCall(IGraphProxy _proxy, bytes calldata _data)\n        external\n        onlyProxyAdmin(_proxy)\n    '
upgrades/GraphProxyStorage.sol, 74, b'    function _admin() internal view returns (address adm) '
upgrades/GraphProxyStorage.sol, 74, b'    function _admin() internal view returns (address adm) '
upgrades/GraphProxyStorage.sol, 87, b'    function _setAdmin(address _newAdmin) internal '
upgrades/GraphProxyStorage.sol, 87, b'    function _setAdmin(address _newAdmin) internal '
