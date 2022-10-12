# Non critical
## [N-1]`require()`/`revert()` statements should have descriptive reason strings
```solidity
GraphProxy.sol:133:        require(success);
GraphProxyAdmin.sol:34:        require(success);
GraphProxyAdmin.sol:47:        require(success);
GraphProxyAdmin.sol:59:        require(success);
```

## [N-2] Use allowlist/denylist rather than blacklist/whitelist
```solidity
L1GraphTokenGateway.sol:35:    mapping(address => bool) public callhookWhitelist;
L1GraphTokenGateway.sol:63:    // Emitted when an address is added to the callhook whitelist
L1GraphTokenGateway.sol:64:    event AddedToCallhookWhitelist(address newWhitelisted);
L1GraphTokenGateway.sol:65:    // Emitted when an address is removed from the callhook whitelist
L1GraphTokenGateway.sol:66:    event RemovedFromCallhookWhitelist(address notWhitelisted);
L1GraphTokenGateway.sol:95:     * - whitelisted callhook callers using addToCallhookWhitelist
L1GraphTokenGateway.sol:148:     * @dev Adds an address to the callhook whitelist.
L1GraphTokenGateway.sol:150:     * @param _newWhitelisted Address to add to the whitelist
L1GraphTokenGateway.sol:152:    function addToCallhookWhitelist(address _newWhitelisted) external onlyGovernor {
L1GraphTokenGateway.sol:153:        require(_newWhitelisted != address(0), "INVALID_ADDRESS");
L1GraphTokenGateway.sol:154:        require(!callhookWhitelist[_newWhitelisted], "ALREADY_WHITELISTED");
L1GraphTokenGateway.sol:155:        callhookWhitelist[_newWhitelisted] = true;
L1GraphTokenGateway.sol:156:        emit AddedToCallhookWhitelist(_newWhitelisted);
L1GraphTokenGateway.sol:160:     * @dev Removes an address from the callhook whitelist.
L1GraphTokenGateway.sol:162:     * @param _notWhitelisted Address to remove from the whitelist
L1GraphTokenGateway.sol:164:    function removeFromCallhookWhitelist(address _notWhitelisted) external onlyGovernor {
L1GraphTokenGateway.sol:165:        require(_notWhitelisted != address(0), "INVALID_ADDRESS");
L1GraphTokenGateway.sol:166:        require(callhookWhitelist[_notWhitelisted], "NOT_WHITELISTED");
L1GraphTokenGateway.sol:167:        callhookWhitelist[_notWhitelisted] = false;
L1GraphTokenGateway.sol:168:        emit RemovedFromCallhookWhitelist(_notWhitelisted);
L1GraphTokenGateway.sol:177:     * Also note that whitelisted senders (some protocol contracts) can include additional calldata
L1GraphTokenGateway.sol:180:     * never succeeds. This requires extra care when adding contracts to the whitelist, but is necessary to ensure that
L1GraphTokenGateway.sol:214:                    extraData.length == 0 || callhookWhitelist[msg.sender] == true,
L1GraphTokenGateway.sol:323:     * @param _data Additional call data for the L2 transaction, which must be empty unless the caller is whitelisted
ICallhookReceiver.sol:6: * be whitelisted by the governor, but also implement this interface that contains
L2GraphTokenGateway.sol:214:     * Note that whitelisted senders (some protocol contracts) can include additional calldata
L2GraphTokenGateway.sol:217:     * never succeeds. This requires extra care when adding contracts to the whitelist, but is necessary to ensure that
L2GraphTokenGateway.sol:224:     * @param _data Extra callhook data, only used when the sender is whitelisted
```
# Low 
## [L-01] `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()`
```solidity
Managed.sol-170-     * @dev Cache a contract address from the Controller registry.
Managed.sol-171-     * @param _name Name of the contract to sync into the cache
Managed.sol-172-     */
Managed.sol-173-    function _syncContract(string memory _name) internal {
Managed.sol:174:        bytes32 nameHash = keccak256(abi.encodePacked(_name));
Managed.sol-175-        address contractAddress = controller.getContractProxy(nameHash);
Managed.sol-176-        if (addressCache[nameHash] != contractAddress) {
Managed.sol-177-            addressCache[nameHash] = contractAddress;
Managed.sol-178-            emit ContractSynced(nameHash, contractAddress);
--
GraphTokenUpgradeable.sol-80-        bytes32 _r,
GraphTokenUpgradeable.sol-81-        bytes32 _s
GraphTokenUpgradeable.sol-82-    ) external {
GraphTokenUpgradeable.sol-83-        bytes32 digest = keccak256(
GraphTokenUpgradeable.sol:84:            abi.encodePacked(
GraphTokenUpgradeable.sol-85-                "\x19\x01",
GraphTokenUpgradeable.sol-86-                DOMAIN_SEPARATOR,
GraphTokenUpgradeable.sol-87-                keccak256(
GraphTokenUpgradeable.sol-88-                    abi.encode(PERMIT_TYPEHASH, _owner, _spender, _value, nonces[_owner], _deadline)
```

