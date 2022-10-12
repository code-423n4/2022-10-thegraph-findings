## Missing zero address validation

If contract miss this zero check address validation there is chance that contract will loose some functionality

     For example:      File:   contracts/governance/Pausable.sol

     55   function _setPauseGuardian(address newPauseGuardian) internal {
     56   address oldPauseGuardian = pauseGuardian;
     57   pauseGuardian = newPauseGuardian;
     58   emit NewPauseGuardian(oldPauseGuardian, pauseGuardian);
     59         }

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L55-L59

Below instances also lacks zero address check.

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L80-L87

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L104-L109

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L69-L74

* hhttps://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L93-L98

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L115-L124

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L130-L139

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L115-L117

## `APPROVE` should be replaced with `SAFEAPPROVE` or `SAFEINCREASEALLOWANCE()`/`SAFEDECREASEALLOWANCE()`

`approve` is subject to a known front-runnning attack. Consider using safeApprove instead.

    File: contracts/gateway/BridgeEscrow.sol   #1
    29        graphToken().approve(_spender, type(uint256).max);

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol#L29

## Use of Block.timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

Navigate to the following contracts.

    File: contracts/governance/Pausable.sol   #1
    32            lastPausePartialTime = block.timestamp;

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L32

      File: contracts/governance/Pausable.sol   #2
      46            lastPauseTime = block.timestamp;

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L46

      File: contracts/l2/token/GraphTokenUpgradeable.sol   #3
      95        require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L95

## `public` functions not called by the contract should be declared `external` instead

Contracts [are allowed](https://docs.soliditylang.org/en/latest/contracts.html#function-overriding) to override their parents’ functions and change the visibility from `external`to `public`

    File: contracts/upgrades/GraphProxyAdmin.sol   #1
    55    function getProxyAdmin(IGraphProxy _proxy) public view returns (address) {

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L55

      File: contracts/upgrades/GraphProxyAdmin.sol   #2
      68    function changeProxyAdmin(IGraphProxy _proxy, address _newAdmin) public onlyGovernor {

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L68

      File: contracts/upgrades/GraphProxyAdmin.sol   #3
      43    function changeProxyAdmin(IGraphProxy _proxy, address _newAdmin) public onlyGovernor {

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L43

      File: contracts/upgrades/GraphProxyAdmin.sol   #4
      30    function getProxyImplementation(IGraphProxy _proxy) public view returns (address) {

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L30

## Return values of `transferFrom()` not checked

Not all `IERC20` implementations `revert()` when there’s a failure in `transfer()`/`transferFrom()`. The function signature has a `boolean` return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually making a payment

    File: contracts/gateway/L1GraphTokenGateway.sol   #1
    235                token.transferFrom(from, escrow, _amount);

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L235

      File: contracts/gateway/L1GraphTokenGateway.sol   #2
      276        token.transferFrom(escrow, _to, _amount);

## `Event` is missing `indexed` fields

Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it’s not necessarily best to index the maximum allowed per event (three fields). Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

    File: contracts/governance/Pausable.sol   #1
    19    event PartialPauseChanged(bool isPaused);

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L19

      File: contracts/governance/Pausable.sol   #2
      20    event PauseChanged(bool isPaused);

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L20

      File: contracts/l2/token/L2GraphToken.sol   #3
      24    event BridgeMinted(address indexed account, uint256 amount);

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L24

      File: contracts/l2/token/L2GraphToken.sol   #4
      26    event BridgeBurned(address indexed account, uint256 amount);

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L26

      File: contracts/l2/token/L2GraphToken.sol   #5
      30    event L1AddressSet(address l1Address);

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L30

            File: contracts/l2/token/L2GraphToken.sol   #6
            28    event GatewaySet(address gateway);

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L28  

## Floating Pragma

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol#L3

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#L3

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L3

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L3

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L3

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L3

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L3

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L3

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L3

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L3

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L3 

## Use a more recent version of solidity

Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol#L3

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#L3

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L3

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L3

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L3

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L3

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L3

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L3

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L3

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L3

* https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L3 

