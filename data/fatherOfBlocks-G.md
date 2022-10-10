**upgrades/GraphUpgradeable**
- L24/32 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L32 - REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS


**governance/Governed.sol**
- L24/41/54 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L55 - In the require it is validated: pendingGovernor != address(0) && msg.sender == pendingGovernor, this generates an unnecessary cost of gas, since validating pendingGovernor != address(0) is not necessary since msg.sender cannot be address(0). This is because the second premise (msg.sender == pendingGovernor) does enough to validate both premises.


**governance/Pausable.sol**
- L31/34/45/48/57/58 - Instead of validating the variable in storage in the if, it is less expensive to validate the input of the function, that is _toPause. The same thing happens when emitting the event, it is less expensive to use the input.


**l2/token/L2GraphToken.sol**
- L36/49/60/70 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.


**upgrades/GraphProxyAdmin.sol**
- L34/47/59 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.


**upgrades/GraphProxy.sol**
- L105/141/142/157 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L105/144 - REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS

- L143 - In the require it is validated: _pendingImplementation != address(0) && msg.sender == _pendingImplementation, this generates an unnecessary cost of gas, since validating _pendingImplementation != address(0) is not necessary since msg.sender cannot be address(0). This is because the second premise (msg.sender == _pendingImplementation) does enough to validate both premises.


**governance/Managed.sol**
- L44/45/49/53/57/104 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.


**gateway/GraphTokenGateway**
- L19/31/40 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L20 - It is less expensive in the require to execute first msg.sender == pauseGuardian, before msg.sender == controller.getGovernor(), since in the call to the controller function a different smart contract is consulted and in the first validation is just a check of a variable.


**gateway/L1GraphTokenGateway.sol**
- L74/78/82/110/111/122/132/142/153/154/165/166/200/201/202/213/217/224/271/275 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L77/78/81/82/223/224/229/242/353/354 - When a variable is created but only used once, less gas would be spent directly using it where it is needed.

- L201/217 - With integers of type uint256 it is less expensive to validate _amount != 0, rather than validating _amount > 0.

- L214 - When we want to validate bools, it is less expensive to validate require(!bool) or require(bool), instead of require(bool == false) or require(bool == true)


**l2/token/GraphTokenUpgradeable.sol**
- L69/98/108/118/145/146/147/148/153/233/234 - Instead of using a require you can use ifs and an error custom, this would generate a lower cost of gas.

- L146/238 - With integers of type uint256 it is less expensive to validate _amount != 0, rather than validating _amount > 0.
