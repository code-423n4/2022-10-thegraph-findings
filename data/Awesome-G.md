## using "> 0" costs more gas than "!= 0"
This change saves about 6 gas per instance. The optimization works until solidity version 0.8.13 where there is a regression in gas costs.

For example:
```
Line 238:        if (_data.length > 0) {
```
[Line 238](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L238)

## Splitting ```require()``` Statements That Use ```&&``` Saves Gas - (Saves ```8``` Gas per ```&&```)
Instead of using the ```&&``` operator in a single require statement to check multiple conditions, using multiple require statements with 1 condition per require statement will save 8 GAS per ```&&```.
for example:
```
Line 142:        require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
```
[Line 142](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L142)

```
Line 142       require(
Line 143:            _pendingImplementation != address(0) && msg.sender == Line 144            _pendingImplementation,
Line 145            "Caller must be the pending implementation");
```
[Line 142-145](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L142-L145)

## Using ```Calldata``` Instead of ```Memory``` for Read-Only Arguments in External Functions Saves Gas
When a function with a ```memory``` array is called externally, the ```abi.decode()``` step has to use a for-loop to copy each index of the ```calldata``` to the ```memory``` index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 * <mem_array>.length). Using ```calldata``` directly, obliviates the need for such a loop in the contract code and runtime execution. Note that even if an interface defines a function as having ```memory``` arguments, it’s still valid for implementation contracts to use ```calldata``` arguments instead.

If the array is passed to an ```internal``` function which passes the array to another internal function where the array is modified and therefore memory is used in the ```external``` call, it’s still more gas-efficient to use ```calldata``` when the external function uses modifiers, since the modifiers may prevent the internal functions from being called. Structs have the same overhead as an array of length one

Note that I’ve also flagged instances where the function is ```public``` but can be marked as ```external``` since it’s not called by the contract, and cases where a constructor is involved
For example:
```
Line 266:         bytes memory _data
```
[Line 266](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L266)

```
Line 286:     function parseOutboundData(bytes memory _data) private view returns (address, bytes memory) {
```
[Line 286](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L286)

## Comparison Operators
It costs less gas to use greater/less than operator (```>```, ```<```) than greater/less than or equal to operator (```>=```, ```<=```) for example:
```
Line 95:        require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
```
[Line 95](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L95)