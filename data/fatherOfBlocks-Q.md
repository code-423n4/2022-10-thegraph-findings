**ALL**
- SE A MORE RECENT VERSION OF SOLIDITY
Use a solidity version of at least 0.8.13 to get the ability to use a list of free functions.


**l2/token/GraphTokenUpgradeable.sol**
- L145 - If the input _l1Token can only have a single value, that is l1GRT, therefore that input could be saved and directly know that it is handled with that address.


**token/IGraphToken.sol**
- L3/5 - The IERC20 import is in pragma 0.8 and the GraphToken interface is created with version 0.7.6, this generates compatibility conflicts.


**curation/ICuration.sol**
- L5 - The IGraphCurationToken interface is imported, but is never used.


**upgrades/GraphProxyAdmin.sol**
- L34/47/59 - If we perform a require and there is a chance that it will revert, it would be beneficial to have a message for that revert.


**upgrades/GraphProxy.sol**
- L21/33 - Instead of the name ifAdmin or ifAdminOrPendingImpl for modifiers, the word "only" instead of if could be a better name.


**governance/Managed.sol**
- L33 - An event, ParameterUpdated, is created and is never used. Therefore it should be removed.


**gateway/L1GraphTokenGateway.sol**
- L142 - It is validated that _escrow != address(0) && Address.isContract(_escrow) but the second validation would already validate that _escrow is not the address(0), since that address is not a contract, therefore it could be just validate that escrow is a contract.

- L200/270/271 - If the input _l1Token can only have a single value, that is graphToken(), therefore that input could be saved and directly know what is handled with that GraphToken.
