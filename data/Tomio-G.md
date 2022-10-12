1.
Title: Using `!=` in `require` statement is more gas efficient

Proof of Concept:
[L1GraphTokenGateway.sol#L201](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L201)
[L1GraphTokenGateway.sol#L217](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L217)

Recommended Mitigation Steps:
Change `> 0` to `!= 0`
________________________________________________________________________

2.
Title: Using multiple `require` instead `&&` can save gas

Proof of Concept:
[L1GraphTokenGateway.sol#L142](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L142)
[Governed.sol#L54-L56](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L54-L56)

Recommended Mitigation Steps:
Change to:

```
	require(_escrow != address(0), "INVALID_ESCROW");
	require(Address.isContract(_escrow), "INVALID_ESCROW");
```
________________________________________________________________________

3.
Title: Boolean comparison

Proof of Concept:
[L1GraphTokenGateway.sol#L214](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L214)

Recommended Mitigation Steps:
Change from `callhookWhitelist[msg.sender] == true` to `callhookWhitelist[msg.sender]`
________________________________________________________________________

4.
Title: abi.encode() is less efficient than abi.encodePacked()

Proof of Concept:
[GraphTokenUpgradeable.sol#L162](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L162)
[L1GraphTokenGateway.sol#L249](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L249)
[L2GraphTokenGateway.sol#L174](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L174)
________________________________________________________________________

5.
Title: Gas improvement on returning `from` and `extraData` value

Proof of Concept:
[L2GraphTokenGateway.sol#L286](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L286)

Recommended Mitigation Steps:
by set `from` and `extraData` in returns L#286 and delete L#287-288 can save gas

```
function parseOutboundData(bytes memory _data) private view returns (address from, bytes memory extraData) { //@audit-info: set here
        if (msg.sender == l2Router) {
            (from, extraData) = abi.decode(_data, (address, bytes));
        } else {
            from = msg.sender;
            extraData = _data;
        }
        return (from, extraData);
    }
```
________________________________________________________________________

6.
Title: Gas improvement on returning `id` value

Proof of Concept:
[GraphTokenUpgradeable.sol#L195](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L195)

Recommended Mitigation Steps:
by set `id` in returns L#195 and delete L#196 can save gas

```
function _getChainID() private pure returns (uint256 id) { //@audit-info: set here
        // solhint-disable-next-line no-inline-assembly
        assembly {
            id := chainid()
        }
        return id;
    }
```
________________________________________________________________________

7.
Title: Expression for `constant` values such as a call to `keccak256()`, should use `immutable` rather than `constant`

Proof of Concept:
[GraphTokenUpgradeable.sol#L34-L37](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L34-L37)
[GraphTokenUpgradeable.sol#L42-L45](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L42-L45)

Recommended Mitigation Steps:
Change from `constant` to `immutable`
reference: [here](https://github.com/ethereum/solidity/issues/9232)
________________________________________________________________________