### Typos
___
[L1GraphTokenGateway.sol: L185](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L185)
```solidity
     * @param _amount Amount of tokens to tranfer
```
Change `tranfer` to `transfer`
___
[L1GraphTokenGateway.sol: L260](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L260)
```solidity
     * @param _to Recepient address on L1
```
Change `Recepient` to `Recipient`
___
___


### Long comment lines
In theory, comments over 80 characters should wrap using multi-line comment syntax. Even if somewhat longer comments (up to 120 characters) are acceptable and a scroll bar is provided, there are cases where very long comment lines interfere with readability. Below are two sets of comments including extra-long lines whose readability could be improved, as shown.
___
[GraphProxy.sol: L65-66](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L65-L66)
```solidity
     * TIP: To get this value clients can read directly from the storage slot shown below (specified by EIP1967) using the
     * https://eth.wiki/json-rpc/API#eth_getstorageat[`eth_getStorageAt`] RPC call.
```
Suggestion:
```solidity
     * TIP: To get this value clients can read directly from the storage slot shown below (specified by EIP1967) 
     * using the https://eth.wiki/json-rpc/API#eth_getstorageat[`eth_getStorageAt`] RPC call.
```
Similarly for the following sets of comments:

[GraphProxy.sol: L78-79](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L78-L79)

[GraphProxy.sol: L91-92](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L91-L92)
___
[L2GraphTokenGateway.sol: L214-219](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L214-L219)
```solidity
     * Note that whitelisted senders (some protocol contracts) can include additional calldata
     * for a callhook to be executed on the L2 side when the tokens are received. In this case, the L2 transaction
     * can revert if the callhook reverts, potentially locking the tokens on the bridge if the callhook
     * never succeeds. This requires extra care when adding contracts to the whitelist, but is necessary to ensure that
     * the tickets can be retried in the case of a temporary failure, and to ensure the atomicity of callhooks
     * with token transfers.
```
Suggestion:
```solidity
     * Note that whitelisted senders (some protocol contracts) can include additional calldata
     * for a callhook to be executed on the L2 side when the tokens are received. In this case, 
     * the L2 transaction can revert if the callhook reverts, potentially locking the tokens on the bridge 
     * if the callhook never succeeds. This requires extra care when adding contracts to the whitelist,
     * but is necessary to ensure that the tickets can be retried in the case of a temporary failure,
     * and to ensure the atomicity of callhooks with token transfers.
```
Similarly for the following set of comments:

[L1GraphTokenGateway.sol: L177-182](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L177-L182)
___
___

### Open items
Code that contains open items should be addressed and modified or removed

[L2GraphTokenGateway.sol: L131-144](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L131-L144)
```solidity
     * @param _l1Token L1 Address of GRT (needed for compatibility with Arbitrum Gateway Router)
     * @param _to Recipient address on L1
     * @param _amount Amount of tokens to burn
     * @param _data Contains sender and additional data (always empty) to send to L1
     * @return ID of the withdraw transaction
     */
    function outboundTransfer(
        address _l1Token,
        address _to,
        uint256 _amount,
        uint256, // unused on L2
        uint256, // unused on L2
        bytes calldata _data
    ) public payable override notPaused nonReentrant returns (bytes memory) {
```
Suggestion: Remove the line `uint256, // unused on L2`, which occurs twice 
___
___
 
### Update sensitive terms in both comments and code
Terms incorporating "black," "white," "slave" or "master" are potentially problematic. Substituting more neutral terminology is becoming [common practice](https://www.zdnet.com/article/mysql-drops-master-slave-and-blacklist-whitelist-terminology/). I see that "blacklist" has been eliminated by using `denylist`. However, the contracts still use `whitelist` and `master`, as shown in the two examples below:

___
[L1GraphTokenGateway.sol: L152-157](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L152-L157)
```solidity
    function addToCallhookWhitelist(address _newWhitelisted) external onlyGovernor {
        require(_newWhitelisted != address(0), "INVALID_ADDRESS");
        require(!callhookWhitelist[_newWhitelisted], "ALREADY_WHITELISTED");
        callhookWhitelist[_newWhitelisted] = true;
        emit AddedToCallhookWhitelist(_newWhitelisted);
    }
```
___
[ICuration.sol: L16](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/ICuration.sol#L16)
```solidity
    function setCurationTokenMaster(address _curationTokenMaster) external;
```
___
Suggestion: Replace `whitelist` with `allowlist`, and `TokenMaster` with `TokenPrimary` or `TokenAdmin`. Similarly for variations of these terms.
___
___

### Capitalization of `require` revert strings should be consistent
For the sake of readability, revert strings should have consistent punctuation throughout `The Graph L2`  

Some error messages are written using ALL CAPS with underscores between words, as the example below shows:

[L2GraphTokenGateway.sol: L98](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L98)
```solidity
        require(_l2Router != address(0), "INVALID_L2_ROUTER");
```
Other error messages are written in a more conversational style without using ALL CAPS or underscores:

[GraphUpgradeable.sol: L24](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L24)
```solidity
        require(msg.sender == _proxy.admin(), "Caller must be the proxy admin");
```
Suggestion: Convert ALL CAP + underscore messages to the more conversational capital + lower case without underscores style
___
___


### Missing `require` revert string
A `require` message should be included to enable users to understand the reason for failure

Recommendation: Add a brief (<= 32 char) explanatory message to each of the four `requires` referenced below. Also, consider using custom error codes (which would be cheaper than revert strings).

___
[GraphProxyAdmin.sol: L30-36](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L30-L36)
```solidity
    function getProxyImplementation(IGraphProxy _proxy) public view returns (address) {
        // We need to manually run the static call since the getter cannot be flagged as view
        // bytes4(keccak256("implementation()")) == 0x5c60da1b
        (bool success, bytes memory returndata) = address(_proxy).staticcall(hex"5c60da1b");
        require(success);
        return abi.decode(returndata, (address));
    }
```
Similarly for the following:

[GraphProxyAdmin.sol: L43-49](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L43-L49)

[GraphProxyAdmin.sol: L55-61](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L55-L61)

[GraphProxy.sol: L129-134](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L129-L134)
___
___

### NatSpec is incomplete
Specific NatSpec statements are missing for the 15 functions below:

[GraphUpgradeable.sol: L48-50](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L48-L50)
```solidity
     * @dev Accept to be an implementation of proxy.
     */
    function acceptProxy(IGraphProxy _proxy) external onlyProxyAdmin(_proxy) {
```
Missing: `@param _proxy`
___
[GraphUpgradeable.sol: L55-59](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L55-L59)
```solidity
     * @dev Accept to be an implementation of proxy and then call a function from the new
     * implementation as specified by `_data`, which should be an encoded function call. This is
     * useful to initialize new storage variables in the proxied contract.
     */
    function acceptProxyAndCall(IGraphProxy _proxy, bytes calldata _data)
```
Missing: `@param _proxy` and `@param _data`
___
[Governed.sol: L29-31](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L29-L31)
```solidity
     * @dev Initialize the governor to the contract caller.
     */
    function _initialize(address _initGovernor) internal {
```
Missing: `@param _initGovernor`
___
[Pausable.sol: L24-26](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L24-L26)
```solidity
     * @notice Change the partial paused state of the contract
     */
    function _setPartialPaused(bool _toPause) internal {
```
Missing: `@param _toPause`
___
[Pausable.sol: L38-40](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L38-L40)
```solidity
     * @notice Change the paused state of the contract
     */
    function _setPaused(bool _toPause) internal {
```
Missing: `@param _toPause`
___
[GraphProxyAdmin.sol: L26-30](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L26-L30)
```solidity
     * @dev Returns the current implementation of a proxy.
     * This is needed because only the proxy admin can query it.
     * @return The address of the current implementation of the proxy.
     */
    function getProxyImplementation(IGraphProxy _proxy) public view returns (address) {
```
Missing: `@param _proxy`
___
[GraphProxyAdmin.sol: L39-43](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L39-L43)
```solidity
     * @dev Returns the pending implementation of a proxy.
     * This is needed because only the proxy admin can query it.
     * @return The address of the pending implementation of the proxy.
     */
    function getProxyPendingImplementation(IGraphProxy _proxy) public view returns (address) {
```
Missing: `@param _proxy`
___
[GraphProxyAdmin.sol: L52-55](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L52-L55)
```solidity
     * @dev Returns the admin of a proxy. Only the admin can query it.
     * @return The address of the current admin of the proxy.
     */
    function getProxyAdmin(IGraphProxy _proxy) public view returns (address) {
```
Missing: `@param _proxy`
___
[GraphProxy.sol: L61-69](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L61-L69)
```solidity
     * @dev Returns the current admin.
     *
     * NOTE: Only the admin and implementation can call this function.
     *
     * TIP: To get this value clients can read directly from the storage slot shown below (specified by EIP1967) using the
     * https://eth.wiki/json-rpc/API#eth_getstorageat[`eth_getStorageAt`] RPC call.
     * `0xb53127684a568b3173ae13b9f8a6016e243e63b6e8ee1178d6a717850b5d6103`
     */
    function admin() external ifAdminOrPendingImpl returns (address) {
```
Missing: `@return`. `@dev` should contain some of the information in the notes so that `@return` is not redundant
___
[GraphProxy.sol: L74-82](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L74-L82)
```solidity
     * @dev Returns the current implementation.
     *
     * NOTE: Only the admin can call this function.
     *
     * TIP: To get this value clients can read directly from the storage slot shown below (specified by EIP1967) using the
     * https://eth.wiki/json-rpc/API#eth_getstorageat[`eth_getStorageAt`] RPC call.
     * `0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc`
     */
    function implementation() external ifAdminOrPendingImpl returns (address) {
```
Missing: `@return`. Again, `@dev` should contain some of the information in the notes so that `@return` is not redundant
___
[GraphProxy.sol: L100-104](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L100-L104)
```solidity
     * @dev Changes the admin of the proxy.
     *
     * NOTE: Only the admin can call this function.
     */
    function setAdmin(address _newAdmin) external ifAdmin {
```
Missing: `@param _newAdmin`
___
[GraphProxy.sol: L127-129](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L127-L129)
```solidity
     * @dev Admin function for new implementation to accept its role as implementation.
     */
    function acceptUpgradeAndCall(bytes calldata data) external ifAdminOrPendingImpl {
```
Missing: `@param data`
___
[Managed.sol: L85-87](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L85-L87)
```solidity
     * @dev Initialize the controller.
     */
    function _initialize(address _controller) internal {
```
Missing: `@param _controller`
___
[Managed.sol: L158-161](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L158-L161)
```solidity
     * @dev Resolve a contract address from the cache or the Controller if not found.
     * @return Address of the contract
     */
    function _resolveContract(bytes32 _nameHash) internal view returns (address) {
```
Missing: `@param _nameHash`
___
[GraphTokenGateway.sol: L52-54](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/GraphTokenGateway.sol#L52-L54)
```solidity
     * @notice Getter to access paused state of this contract
     */
    function paused() external view returns (bool) {
```
Missing: `@return` 
___
___


### NatSpec statements are missing
NatSpec statements are entirely missing for the functions listed below. In some cases, a group of functions is proceeded by a comment such as `// -- Configuration --` or ` // -- Getters --`. However, such comments, which serve as explanatory headings for the functions, are not as helpful as would be `@notice` statements (to explain to end users what a function does) and `@param` and `@return` statements (which describe the roles of individual variables).  

**Managed.sol**

[L43](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L43);     [L48](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L48);     [L52](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L52);     [L56](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L56)

**IGraphCurationToken.sol**

[L8](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/IGraphCurationToken.sol#L8);   [L10](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/IGraphCurationToken.sol#L10);   [L12](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/IGraphCurationToken.sol#L12)   

**IGraphProxy.sol**

[L6](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/IGraphProxy.sol#L6);   [L8](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/IGraphProxy.sol#L8);   [L10](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/IGraphProxy.sol#L10);   [L12](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/IGraphProxy.sol#L12);   [L14](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/IGraphProxy.sol#L14);   [L16](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/IGraphProxy.sol#L16);   [L18](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/IGraphProxy.sol#L18)

**IEpochManager.sol**

[L8](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/epochs/IEpochManager.sol#L8);   [L12](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/epochs/IEpochManager.sol#L12);    [L16](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/epochs/IEpochManager.sol#L16);    [L18](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/epochs/IEpochManager.sol#L18);    [L20](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/epochs/IEpochManager.sol#L20);    [L22](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/epochs/IEpochManager.sol#L22);    [L24](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/epochs/IEpochManager.sol#L24);  [L26](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/epochs/IEpochManager.sol#L26);  [L28](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/epochs/IEpochManager.sol#L28);   [L30](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/epochs/IEpochManager.sol#L30)         

**IController.sol**

[L6](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/IController.sol#L6);   [L10](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/IController.sol#L10);   [L12](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/IController.sol#L12);   [L14](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/IController.sol#L14);   [L16](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/IController.sol#L16);   [L20](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/IController.sol#L20);   [L22](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/IController.sol#L22);   [L24](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/IController.sol#L24);   [L26](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/IController.sol#L26);   [L28](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/IController.sol#L28)

**IGraphToken.sol**

[L10](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/token/IGraphToken.sol#L10);   [L12](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/token/IGraphToken.sol#L12);   [L14](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/token/IGraphToken.sol#L14);   [L18](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/token/IGraphToken.sol#L18);   [L20](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/token/IGraphToken.sol#L20);   [L22](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/token/IGraphToken.sol#L22);   [L24](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/token/IGraphToken.sol#L24);   [L28-36](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/token/IGraphToken.sol#L28-L36);   [L40](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/token/IGraphToken.sol#L40);   [L42](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/token/IGraphToken.sol#L42)

**IRewardsManager.sol**

[L18](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/IRewardsManager.sol#L18);   [L20](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/IRewardsManager.sol#L20);   [L24](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/IRewardsManager.sol#L24);   [L26](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/IRewardsManager.sol#L26);   [L28](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/IRewardsManager.sol#L28);   [L31](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/IRewardsManager.sol#L31);   [L35](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/IRewardsManager.sol#L35);   [L37](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/IRewardsManager.sol#L37);   [L39-42](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/IRewardsManager.sol#L39-L42);   [L44-47](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/IRewardsManager.sol#L44-L47);   [L49](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/IRewardsManager.sol#L49);   [L53](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/IRewardsManager.sol#L53);   [L55](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/IRewardsManager.sol#L55);   [L59](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/IRewardsManager.sol#L59);   [L61](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/IRewardsManager.sol#L61)

**ICuration.sol**

[L10](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/ICuration.sol#L10);   [L12](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/ICuration.sol#L12);   [L14](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/ICuration.sol#L14);   [L16](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/ICuration.sol#L16);   [L20-24](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/ICuration.sol#L20-L24);   [L26-30](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/ICuration.sol#L26-L30);   [L32](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/ICuration.sol#L32);   [L36](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/ICuration.sol#L36);   [L38-41](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/ICuration.sol#L38-L41);   [L43](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/ICuration.sol#L43);   [L45](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/ICuration.sol#L45);   [L47-50](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/ICuration.sol#L47-L50);   [L52-55](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/ICuration.sol#L52-L55);   [L57](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/ICuration.sol#L57)

**IStaking.sol**

[L30](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L30);   [L32](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L32);   [L34](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L34);   [L36](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L36);   [L38](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L38);   [L40](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L40);   [L42](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L42);   [L44](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L44);   [L46-50](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L46-L50);   [L52](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L52);   [L54](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L54);   [L56](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L56);   [L58](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L58);   [L60](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L60);   [L64](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L64);   [L66](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L66);   [L70](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L70);   [L72](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L72);   [L74](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L74);   [L76-81](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L76-L81);   [L83](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L83);   [L85](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L85);   [L89](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L89);   [L91](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L91);   [L93](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L93);   [L97-103](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L97-L103);   [L105-112](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L105-L112);   [L114](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L114);   [L116](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L116);   [L118-127](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L118-L127);   [L129](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L129);   [L131](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L131);   [L133](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L133);   [L137](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L137);   [L139](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L139);   [L141](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L141);   [L143](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L143);   [L145](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L145);   [L147](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L147);   [L149-152](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L149-L152);   [L154-157](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L154-L157);  [L159](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L159) 
___
___
