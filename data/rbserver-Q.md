## [L-01] PROXY'S ADMIN CAN BE SET TO WRONG ADDRESS
When the following `setAdmin` function in the `GraphProxy` contract is called, `require(_newAdmin != address(0), ...)` is executed. However, this does not prevent setting the proxy's admin to a wrong address. If the admin of the proxy is set to a malicious address, actions like re-initializing the implementation to use a malicious controller can be performed. To avoid the proxy's admin from being locked and decrease the potential attack surface, a two-step procedure for setting the proxy's admin can be used instead of directly setting it through calling `setAdmin`; using this approach, if the wrong address is set to the pending admin, the current admin can immediately call the function, which is the first step, to set the pending admin to the correct address before the wrong address accepts the admin role.

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L104-L107
```solidity
    function setAdmin(address _newAdmin) external ifAdmin {
        require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
        _setAdmin(_newAdmin);
    }
```

## [L-02] `ArbRetryableTx.getSubmissionPrice` CAN BE QUERIED AND CHECKED AGAINST IN `L1GraphTokenGateway.outboundTransfer` FUNCTION
When calling the following `outboundTransfer` function in the `L1GraphTokenGateway` contract, `uint256 expectedEth = maxSubmissionCost.add(_maxGas.mul(_gasPriceBid)); require(msg.value >= expectedEth, "WRONG_ETH_VALUE");` is executed. Because `maxSubmissionCost` can be arbitrarily encoded in the `_data` input, this `require` statement does not guarantee that the decoded `maxSubmissionCost` covers the actual base submission fee. Sending ETH that does not cover the actual base submission fee will cause the Retryable Ticket creation to fail and break the atomicity of the L1 to L2 transactions. To prevent this, `ArbRetryableTx.getSubmissionPrice` can be queried to get the current base submission fee. As mentioned in https://github.com/OffchainLabs/arbitrum/blob/master/docs/L1_L2_Messages.md#important-note-about-base-submission-fee, the returned current base submission fee can increase once every 24 hour period by at most 50% of its current value, and any amount overpaid will be credited to the specified credit-back-address; hence, before ensuring that the provided ETH is enough, a `require` statement can be added in this `outboundTransfer` function to verify that `maxSubmissionCost` is at least 1.5 times of the current base submission fee returned by `ArbRetryableTx.getSubmissionPrice`.

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L191-L250
```solidity
    function outboundTransfer(
        address _l1Token,
        address _to,
        uint256 _amount,
        uint256 _maxGas,
        uint256 _gasPriceBid,
        bytes calldata _data
    ) external payable override notPaused returns (bytes memory) {
        ...
        {
            uint256 maxSubmissionCost;
            bytes memory outboundCalldata;
            {
                bytes memory extraData;
                (from, maxSubmissionCost, extraData) = parseOutboundData(_data);
                require(
                    extraData.length == 0 || callhookWhitelist[msg.sender] == true,
                    "CALL_HOOK_DATA_NOT_ALLOWED"
                );
                require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");

                {
                    // makes sure only sufficient ETH is supplied as required for successful redemption on L2
                    // if a user does not desire immediate redemption they should provide
                    // a msg.value of AT LEAST maxSubmissionCost
                    uint256 expectedEth = maxSubmissionCost.add(_maxGas.mul(_gasPriceBid));
                    require(msg.value >= expectedEth, "WRONG_ETH_VALUE");
                }
                outboundCalldata = getOutboundCalldata(_l1Token, from, _to, _amount, extraData);
            }
            {
                L2GasParams memory gasParams = L2GasParams(
                    maxSubmissionCost,
                    _maxGas,
                    _gasPriceBid
                );
                // transfer tokens to escrow
                token.transferFrom(from, escrow, _amount);
                seqNum = sendTxToL2(
                    inbox,
                    l2Counterpart,
                    from,
                    msg.value,
                    0,
                    gasParams,
                    outboundCalldata
                );
            }
        }
        ...
    }
```

## [L-03] VULNERABILITIES IN `@openzeppelin/contracts 3.4.1` AND `@openzeppelin/contracts-upgradeable 3.4.2`
As shown in the following code in `package.json`, version 3.4.1 of `@openzeppelin/contracts` and version 3.4.2 of `@openzeppelin/contracts-upgradeable` can be used. For these versions, there are vulnerabilities related to `ECDSA.recover`, `initializer`, etc. It looks like that the code in scope are not affected by these vulnerabilities but the protocol team should be aware of them.

Please see the following links for reference:
- https://security.snyk.io/package/npm/@openzeppelin%2Fcontracts/3.4.1
- https://security.snyk.io/package/npm/@openzeppelin%2Fcontracts-upgradeable/3.4.2

To reduce potential attack surface and be more future-proofed, please consider upgrading these packages to at least version 4.7.3.

https://github.com/code-423n4/2022-10-thegraph/blob/main/package.json#L27-L28
```solidity
    "@openzeppelin/contracts": "^3.4.1",
    "@openzeppelin/contracts-upgradeable": "3.4.2",
```

## [L-04] MISSING `address(0)` CHECKS FOR CRITICAL ADDRESS INPUTS
To prevent unintended behaviors, critical address inputs should be checked against `address(0)`.

Please consider checking `_impl` and `_admin` in the following constructor.
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L46-L58
```solidity
    constructor(address _impl, address _admin) {
        assert(ADMIN_SLOT == bytes32(uint256(keccak256("eip1967.proxy.admin")) - 1));
        assert(
            IMPLEMENTATION_SLOT == bytes32(uint256(keccak256("eip1967.proxy.implementation")) - 1)
        );
        assert(
            PENDING_IMPLEMENTATION_SLOT ==
                bytes32(uint256(keccak256("eip1967.proxy.pendingImplementation")) - 1)
        );

        _setAdmin(_admin);
        _setPendingImplementation(_impl);
    }
```

## [N-01] MISSING INDEXED EVENT FIELD
Querying event can be optimized with index. Please consider adding `indexed` to the relevant field of the following event.

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L56
```solidity
    event ArbitrumAddressesSet(address inbox, address l1Router);
```

## [N-02] MISSING REASON STRINGS IN `require` STATEMENTS
When the reason strings are missing in the `require` statements, it is unclear about why certain conditions revert. Please add descriptive reason strings for the following `require` statements.
```solidity
contracts\upgrades\GraphProxyAdmin.sol
  34: require(success);
  47: require(success);
  59: require(success);
```

## [N-03] INCOMPLETE NATSPEC COMMENTS
NatSpec comments provide rich code documentation. @param and/or @return comments are missing for the following functions. Please consider completing NatSpec comments for them.
```solidity
contracts\gateway\GraphTokenGateway.sol
  54: function paused() external view returns (bool) {  

contracts\governance\Governed.sol
  31: function _initialize(address _initGovernor) internal {

contracts\governance\Managed.sol
  87: function _initialize(address _controller) internal {  
  161: function _resolveContract(bytes32 _nameHash) internal view returns (address) {

contracts\governance\Pausable.sol
  40: function _setPaused(bool _toPause) internal { 

contracts\upgrades\GraphProxy.sol
  129: function acceptUpgradeAndCall(bytes calldata data) external ifAdminOrPendingImpl {

contracts\upgrades\GraphUpgradeable.sol
  59: function acceptProxyAndCall(IGraphProxy _proxy, bytes calldata _data) 
```

## [N-04] MISSING NATSPEC COMMENTS
NatSpec comments provide rich code documentation. NatSpec comments are missing for the following functions. Please consider adding them.
```solidity
contracts\governance\Managed.sol
  43: function _notPartialPaused() internal view {
  48: function _notPaused() internal view virtual {
  52: function _onlyGovernor() internal view {
  56: function _onlyController() internal view {
```

## [N-05] FLOATING PRAGMAS
It is a best practice to lock pragmas instead of using floating pragmas to ensure that contracts are tested and deployed with the intended compiler version. Accidentally deploying contracts with different compiler versions can lead to unexpected risks and undiscovered bugs. Please consider locking pragma for the following contracts.

```solidity
contracts\gateway\GraphTokenGateway.sol
  3: pragma solidity ^0.7.6;

contracts\gateway\L1GraphTokenGateway.sol
  3: pragma solidity ^0.7.6;

contracts\governance\Governed.sol
  3: pragma solidity ^0.7.6;

contracts\governance\Managed.sol
  3: pragma solidity ^0.7.6;

contracts\governance\Pausable.sol
  3: pragma solidity ^0.7.6;

contracts\l2\gateway\L2GraphTokenGateway.sol
  3: pragma solidity ^0.7.6;

contracts\l2\token\GraphTokenUpgradeable.sol
  3: pragma solidity ^0.7.6;

contracts\l2\token\L2GraphToken.sol
  3: pragma solidity ^0.7.6;

contracts\upgrades\GraphProxy.sol
  3: pragma solidity ^0.7.6;

contracts\upgrades\GraphProxyAdmin.sol
  3: pragma solidity ^0.7.6;

contracts\upgrades\GraphProxyStorage.sol
  3: pragma solidity ^0.7.6;

contracts\upgrades\GraphUpgradeable.sol
  3: pragma solidity ^0.7.6;
```