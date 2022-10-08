## Summary<a name="Summary">

### Low Risk Issues
| |Issue|Instances|
|-|:-|:-:|
| [LOW&#x2011;1](#LOW&#x2011;1) | Missing Checks for Address(0x0)  | 22 |
| [LOW&#x2011;2](#LOW&#x2011;2) | Use Safetransfer Instead Of Transfer  | 2 |
| [LOW&#x2011;3](#LOW&#x2011;3) | IERC20 `approve()` Is Deprecated | 1 |
| [LOW&#x2011;4](#LOW&#x2011;4) | The Contract Should `approve(0)` First | 1 |
| [LOW&#x2011;5](#LOW&#x2011;5) | Use `_safeMint` instead of `_mint` | 3 |
| [LOW&#x2011;6](#LOW&#x2011;6) | Vulnerable To Cross-chain Replay Attacks Due To Static DOMAIN_SEPARATOR | 1 |
| [LOW&#x2011;7](#LOW&#x2011;7) | Missing Contract-existence Checks Before Low-level Calls | 2 |
| [LOW&#x2011;8](#LOW&#x2011;8) | Contracts are not using their OZ Upgradeable counterparts | 8 |
| [LOW&#x2011;9](#LOW&#x2011;9) | Critical Changes Should Use Two-step Procedure | 14 |
| [LOW&#x2011;10](#LOW&#x2011;10) | Low Level Calls With Solidity Version 0.8.14 Can Result In Optimiser Bug | 9 |
| [LOW&#x2011;11](#LOW&#x2011;11) | Missing parameter validation | 2 |
| [LOW&#x2011;12](#LOW&#x2011;12) | Upgrade OpenZeppelin Contract Dependency | 2 |

Total: 67 instances over 12 issues

### Non-critical Issues
| |Issue|Instances|
|-|:-|:-:|
| [NC&#x2011;1](#NC&#x2011;1) | Use a more recent version of Solidity | 23 |
| [NC&#x2011;2](#NC&#x2011;2) | Event Is Missing Indexed Fields | 1 |
| [NC&#x2011;3](#NC&#x2011;3) | Public Functions Not Called By The Contract Should Be Declared External Instead | 2 |
| [NC&#x2011;4](#NC&#x2011;5) | `require()` / `revert()` Statements Should Have Descriptive Reason Strings | 4 |
| [NC&#x2011;5](#NC&#x2011;6) | Implementation contract may not be initialized | 2 |
| [NC&#x2011;6](#NC&#x2011;7) | Avoid Floating Pragmas: The Version Should Be Locked | 20 |
| [NC&#x2011;7](#NC&#x2011;8) | Use of Block.Timestamp | 3 |
| [NC&#x2011;8](#NC&#x2011;10) | Non-usage of specific imports | 32 |
| [NC&#x2011;9](#NC&#x2011;11) | Expressions for constant values such as a call to `keccak256()`, should use `immutable` rather than `constant` | 4 |
| [NC&#x2011;10](#NC&#x2011;12) | Use `bytes.concat()` | 2 |

Total: 93 instances over 12 issues

## Low Risk Issues

### <a href="#Summary">[LOW&#x2011;1]</a><a name="LOW&#x2011;1"> Missing Checks for Address(0x0) 

Lack of zero-address validation on address parameters may lead to transaction reverts, waste gas, require resubmission of transactions and may even force contract redeployments in certain cases within the protocol.

#### <ins>Proof Of Concept</ins>


```
function initialize: address _controller
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/BridgeEscrow.sol#L20

```
function approveAll: address _spender
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/BridgeEscrow.sol#L28

```
function revokeAll: address _spender
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/BridgeEscrow.sol#L36

```
function initialize: address _controller
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L99

```
function finalizeInboundTransfer: address _from
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L263

```
function finalizeInboundTransfer: address _to
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L263

```
function setController: address _controller
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L95

```
function initialize: address _controller
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L87

```
function outboundTransfer: address _l1Token
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L188

```
function outboundTransfer: address _to
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L188

```
function finalizeInboundTransfer: address _from
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L226

```
function finalizeInboundTransfer: address _to
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L226

```
function permit: address _spender
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L74

```
function removeMinter: address _account
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L114

```
function mint: address _to
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L132

```
function _addMinter: address _account
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L177

```
function _removeMinter: address _account
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L186

```
function bridgeMint: address _account
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol#L80

```
function bridgeBurn: address _account
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol#L90

```
function upgradeTo: address _newImplementation
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L115

```
function changeProxyAdmin: address _newAdmin
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L68

```
function upgrade: address _implementation
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L77



#### <ins>Recommended Mitigation Steps</ins>

Consider adding explicit zero-address validation on input parameters of address type.



### <a href="#Summary">[LOW&#x2011;2]</a><a name="LOW&#x2011;2"> Use Safetransfer Instead Of Transfer 

It is good to add a `require()` statement that checks the return value of token transfers or to use something like OpenZeppelin’s `safeTransfer`/`safeTransferFrom` unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract.

For example, Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens. For example Tether (USDT)'s transfer() and transferFrom() functions do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to IERC20, their function signatures do not match and therefore the calls made, revert.

#### <ins>Proof Of Concept</ins>


```
token.transferFrom(from, escrow, _amount);
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L235

```
token.transferFrom(escrow, _to, _amount);
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L276



#### <ins>Recommended Mitigation Steps</ins>

Consider using `safeTransfer`/`safeTransferFrom` or `require()` consistently.



### <a href="#Summary">[LOW&#x2011;3]</a><a name="LOW&#x2011;3"> IERC20 `approve()` Is Deprecated

`approve()` is subject to a known front-running attack. It is deprecated in favor of `safeIncreaseAllowance()` and `safeDecreaseAllowance()`. If only setting the initial allowance to the value that means infinite, `safeIncreaseAllowance()` can be used instead.

https://docs.openzeppelin.com/contracts/3.x/api/token/erc20#IERC20-approve-address-uint256-

#### <ins>Proof Of Concept</ins>

```
graphToken().approve(_spender, type(uint256).max);
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/BridgeEscrow.sol#L29



#### <ins>Recommended Mitigation Steps</ins>

Consider using `safeIncreaseAllowance()` / `safeDecreaseAllowance()` instead.



### <a href="#Summary">[LOW&#x2011;4]</a><a name="LOW&#x2011;4"> The Contract Should `approve(0)` First

Some tokens (like USDT L199) do not work when changing the allowance from an existing non-zero allowance value. They must first be approved by zero and then the actual allowance must be approved.

#### <ins>Proof Of Concept</ins>


```
graphToken().approve(_spender, type(uint256).max);
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/BridgeEscrow.sol#L29



#### <ins>Recommended Mitigation Steps</ins>

Approve with a zero amount first before setting the actual amount.



### <a href="#Summary">[LOW&#x2011;5]</a><a name="LOW&#x2011;5"> Use `_safeMint` instead of `_mint`

According to openzepplin's ERC721, the use of `_mint` is discouraged, use _safeMint whenever possible.
https://docs.openzeppelin.com/contracts/3.x/api/token/erc721#ERC721-_mint-address-uint256-

#### <ins>Proof Of Concept</ins>


```
_mint(_to, _amount);
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L133

```
_mint(_owner, _initialSupply);
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L155

```
_mint(_account, _amount);
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol#L81



#### <ins>Recommended Mitigation Steps</ins>

Use `_safeMint` whenever possible instead of `_mint`



### <a href="#Summary">[LOW&#x2011;6]</a><a name="LOW&#x2011;6"> Vulnerable To Cross-chain Replay Attacks Due To Static DOMAIN_SEPARATOR

The protocol calculates the chainid it should assign during its execution and permanently stores it in an immutable variable. Should Ethereum fork in the feature, the chainid will change however the one used by the permits will not enabling a user to use any new permits on both chains thus breaking the token on the forked chain permanently.

Please consult EIP1344 for more details: https://eips.ethereum.org/EIPS/eip-1344#rationale

#### <ins>Proof Of Concept</ins>

```
        DOMAIN_SEPARATOR = keccak256(
            abi.encode(
                DOMAIN_TYPE_HASH,
                DOMAIN_NAME_HASH,
                DOMAIN_VERSION_HASH,
                _getChainID(),
                address(this),
                DOMAIN_SALT
            )
        );
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L50



#### <ins>Recommended Mitigation Steps</ins>

The mitigation action that should be applied is the calculation of the chainid dynamically on each permit invocation. As a gas optimization, the deployment pre-calculated hash for the permits can be stored to an immutable variable and a validation can occur on the permit function that ensure the current chainid is equal to the one of the cached hash and if not, to re-calculate it on the spot.



### <a href="#Summary">[LOW&#x2011;7]</a><a name="LOW&#x2011;7"> Missing Contract-existence Checks Before Low-level Calls

Low-level calls return success if there is no code present at the specified address. 

#### <ins>Proof Of Concept</ins>


```
(bool success, ) = _implementation().delegatecall(data);
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L132

```
let result := delegatecall(gas(), impl, ptr, calldatasize(), 0, 0)
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L170




#### <ins>Recommended Mitigation Steps</ins>

In addition to the zero-address checks, add a check to verify that `<address>.code.length > 0`



### <a href="#Summary">[LOW&#x2011;8]</a><a name="LOW&#x2011;8"> Contracts are not using their OZ Upgradeable counterparts

The non-upgradeable standard version of OpenZeppelin’s library are inherited / used by the contracts.
It would be safer to use the upgradeable versions of the library contracts to avoid unexpected behaviour.

#### <ins>Proof of Concept</ins>

```
import "@openzeppelin/contracts/utils/Address.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L6

```
import "@openzeppelin/contracts/math/SafeMath.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L7

```
import "@openzeppelin/contracts/math/SafeMath.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L7

```
import "@openzeppelin/contracts/cryptography/ECDSA.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L6

```
import "@openzeppelin/contracts/math/SafeMath.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L7

```
import "@openzeppelin/contracts/math/SafeMath.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol#L5

```
import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/token/IGraphToken.sol#L5

```
import "@openzeppelin/contracts/utils/Address.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L5



#### <ins>Recommended Mitigation Steps</ins>

Where applicable, use the contracts from `@openzeppelin/contracts-upgradeable` instead of `@openzeppelin/contracts`.



### <a href="#Summary">[LOW&#x2011;9]</a><a name="LOW&#x2011;9"> Critical Changes Should Use Two-step Procedure

The critical procedures should be two step process.

See similar findings in previous Code4rena contests for reference:
https://code4rena.com/reports/2022-06-illuminate/#2-critical-changes-should-use-two-step-procedure

#### <ins>Proof Of Concept</ins>

```
function setPauseGuardian(address _newPauseGuardian) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/GraphTokenGateway.sol#L30

```
function setPaused(bool _newPaused) external onlyGovernorOrGuardian {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/GraphTokenGateway.sol#L47

```
function setArbitrumAddresses(address _inbox, address _l1Router) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L109

```
function setL2TokenAddress(address _l2GRT) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L121

```
function setL2CounterpartAddress(address _l2Counterpart) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L131

```
function setEscrowAddress(address _escrow) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L141

```
function setController(address _controller) external onlyController {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L95

```
function setL2Router(address _l2Router) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L97

```
function setL1TokenAddress(address _l1GRT) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L107

```
function setL1CounterpartAddress(address _l1Counterpart) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L117

```
function setGateway(address _gw) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol#L59

```
function setL1Address(address _addr) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol#L69

```
function setAdmin(address _newAdmin) external ifAdmin {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L104

```
function changeProxyAdmin(IGraphProxy _proxy, address _newAdmin) public onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L68



#### <ins>Recommended Mitigation Steps</ins>

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.



### <a href="#Summary">[LOW&#x2011;10]</a><a name="LOW&#x2011;10"> Low Level Calls With Solidity Version 0.8.14 Can Result In Optimiser Bug

The project contracts in scope are using low level calls with solidity version before 0.8.14 which can result in optimizer bug.
https://medium.com/certora/overly-optimistic-optimizer-certora-bug-disclosure-2101e3f7994d

Simliar findings in Code4rena contests for reference:
https://code4rena.com/reports/2022-06-illuminate/#5-low-level-calls-with-solidity-version-0814-can-result-in-optimiser-bug

#### <ins>Proof Of Concept</ins>

POC can be found in the above medium reference url.

Functions that execute low level calls in contracts with solidity version under 0.8.14

```
function _getChainID() private pure returns (uint256) {
        uint256 id;
                assembly {
            id := chainid()
        }
        return id;
    }
}
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L195

```
function _fallback() internal {
        require(msg.sender != _admin(), "Cannot fallback to proxy target");

        assembly {
                        let ptr := mload(0x40)

                        let impl := and(sload(IMPLEMENTATION_SLOT), 0xffffffffffffffffffffffffffffffffffffffff)

                        calldatacopy(ptr, 0, calldatasize())

                        let result := delegatecall(gas(), impl, ptr, calldatasize(), 0, 0)
            let size := returndatasize()

                        returndatacopy(ptr, 0, size)

                        switch result
            case 0 {
                revert(ptr, size)
            }
            default {
                return(ptr, size)
            }
        }
    }

    
    fallback() external payable {
        _fallback();
    }

    
    receive() external payable {
        _fallback();
    }
}
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L156

```
function _admin() internal view returns (address adm) {
        bytes32 slot = ADMIN_SLOT;
        assembly {
            adm := sload(slot)
        }
    }
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyStorage.sol#L69

```
function _setAdmin(address _newAdmin) internal {
        bytes32 slot = ADMIN_SLOT;
        assembly {
            sstore(slot, _newAdmin)
        }

        emit AdminUpdated(_admin(), _newAdmin);
    }
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyStorage.sol#L80

```
function _implementation() internal view returns (address impl) {
        bytes32 slot = IMPLEMENTATION_SLOT;
        assembly {
            impl := sload(slot)
        }
    }
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyStorage.sol#L93

```
function _pendingImplementation() internal view returns (address impl) {
        bytes32 slot = PENDING_IMPLEMENTATION_SLOT;
        assembly {
            impl := sload(slot)
        }
    }
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyStorage.sol#L104

```
function _setImplementation(address _newImplementation) internal {
        address oldImplementation = _implementation();

        bytes32 slot = IMPLEMENTATION_SLOT;
        assembly {
            sstore(slot, _newImplementation)
        }

        emit ImplementationUpdated(oldImplementation, _newImplementation);
    }
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyStorage.sol#L115

```
function _setPendingImplementation(address _newImplementation) internal {
        address oldPendingImplementation = _pendingImplementation();

        bytes32 slot = PENDING_IMPLEMENTATION_SLOT;
        assembly {
            sstore(slot, _newImplementation)
        }

        emit PendingImplementationUpdated(oldPendingImplementation, _newImplementation);
    }
}
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyStorage.sol#L130

```
function _implementation() internal view returns (address impl) {
        bytes32 slot = IMPLEMENTATION_SLOT;
        assembly {
            impl := sload(slot)
        }
    }
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphUpgradeable.sol#L40





#### <ins>Recommended Mitigation Steps</ins>

Consider upgrading to at least solidity v0.8.15.



### <a href="#Summary">[LOW&#x2011;11]</a><a name="LOW&#x2011;11"> Missing parameter validation

Some parameters of constructors are not checked for invalid values.

#### <ins>Proof Of Concept</ins>

```
address _impl
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L46

```
address _admin
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L46



#### <ins>Recommended Mitigation Steps</ins>

Validate the parameters.



### <a href="#Summary">[LOW&#x2011;12]</a><a name="LOW&#x2011;12"> Upgrade OpenZeppelin Contract Dependency

An outdated OZ version is used (which has known vulnerabilities, see: https://github.com/OpenZeppelin/openzeppelin-contracts/security/advisories).

#### <ins>Proof Of Concept</ins>


```
@openzeppelin/contracts: ^3.4.1
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/package.json#L27

```
@openzeppelin/contracts-upgradeable: 3.4.2
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/package.json#L28



#### <ins>Recommended Mitigation Steps</ins>

Update OpenZeppelin Contracts Usage in package.json



## Non Critical Issues

### <a href="#Summary">[NC&#x2011;1]</a><a name="NC&#x2011;1"> Use a more recent version of Solidity

Use a solidity version of at least 0.8.4 to get bytes.concat() instead of abi.encodePacked(<bytes>,<bytes>)
Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>)
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions

#### <ins>Proof Of Concept</ins>


```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/ICuration.sol#L3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/IGraphCurationToken.sol#L3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/epochs/IEpochManager.sol#L3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/BridgeEscrow.sol#L3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/GraphTokenGateway.sol#L3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/ICallhookReceiver.sol#L9

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Governed.sol#L3

```
pragma solidity >=0.6.12 <0.8.0;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/IController.sol#L3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Pausable.sol#L3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol#L3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/rewards/IRewardsManager.sol#L3

```
pragma solidity >=0.6.12 <0.8.0;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L3

```
pragma solidity >=0.6.12 <0.8.0;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStakingData.sol#L3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/token/IGraphToken.sol#L3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyStorage.sol#L3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphUpgradeable.sol#L3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/IGraphProxy.sol#L3



#### <ins>Recommended Mitigation Steps</ins>

Consider updating to a more recent solidity version.



### <a href="#Summary">[NC&#x2011;2]</a><a name="NC&#x2011;2"> Event Is Missing Indexed Fields

Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). 

Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

#### <ins>Proof Of Concept</ins>


```
event ArbitrumAddressesSet(address inbox, address l1Router);
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L56






### <a href="#Summary">[NC&#x2011;3]</a><a name="NC&#x2011;3"> Public Functions Not Called By The Contract Should Be Declared External Instead

Contracts are allowed to override their parents’ functions and change the visibility from external to public.

#### <ins>Proof Of Concept</ins>


```
function changeProxyAdmin(IGraphProxy _proxy, address _newAdmin) public onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L68

```
function upgrade(IGraphProxy _proxy, address _implementation) public onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L77




### <a href="#Summary">[NC&#x2011;4]</a><a name="NC&#x2011;4"> `require()` / `revert()` Statements Should Have Descriptive Reason Strings

#### <ins>Proof Of Concept</ins>


```
require(success);
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L132

```
require(success);
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L32

```
require(success);
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L45

```
require(success);
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L57






### <a href="#Summary">[NC&#x2011;5]</a><a name="NC&#x2011;5"> Implementation contract may not be initialized

OpenZeppelin recommends that the initializer modifier be applied to constructors. 
Per OZs Post implementation contract should be initialized to avoid potential griefs or exploits.
https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/5

#### <ins>Proof Of Concept</ins>


```
constructor(address _impl, address _admin) {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L46

```
constructor() { 
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L21





### <a href="#Summary">[NC&#x2011;6]</a><a name="NC&#x2011;6"> Avoid Floating Pragmas: The Version Should Be Locked

Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

#### <ins>Proof Of Concept</ins>

```
Found usage of floating pragmas ^0.7.6 of Solidity in [ICuration.sol]
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/ICuration.sol#L3

```
Found usage of floating pragmas ^0.7.6 of Solidity in [IGraphCurationToken.sol]
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/IGraphCurationToken.sol#L3

```
Found usage of floating pragmas ^0.7.6 of Solidity in [IEpochManager.sol]
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/epochs/IEpochManager.sol#L3

```
Found usage of floating pragmas ^0.7.6 of Solidity in [BridgeEscrow.sol]
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/BridgeEscrow.sol#L3

```
Found usage of floating pragmas ^0.7.6 of Solidity in [GraphTokenGateway.sol]
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/GraphTokenGateway.sol#L3

```
Found usage of floating pragmas ^0.7.6 of Solidity in [ICallhookReceiver.sol]
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/ICallhookReceiver.sol#L9

```
Found usage of floating pragmas ^0.7.6 of Solidity in [L1GraphTokenGateway.sol]
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L3

```
Found usage of floating pragmas ^0.7.6 of Solidity in [Governed.sol]
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Governed.sol#L3

```
Found usage of floating pragmas ^0.7.6 of Solidity in [Managed.sol]
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L3

```
Found usage of floating pragmas ^0.7.6 of Solidity in [Pausable.sol]
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Pausable.sol#L3

```
Found usage of floating pragmas ^0.7.6 of Solidity in [L2GraphTokenGateway.sol]
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L3

```
Found usage of floating pragmas ^0.7.6 of Solidity in [GraphTokenUpgradeable.sol]
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L3

```
Found usage of floating pragmas ^0.7.6 of Solidity in [L2GraphToken.sol]
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol#L3

```
Found usage of floating pragmas ^0.7.6 of Solidity in [IRewardsManager.sol]
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/rewards/IRewardsManager.sol#L3

```
Found usage of floating pragmas ^0.7.6 of Solidity in [IGraphToken.sol]
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/token/IGraphToken.sol#L3

```
Found usage of floating pragmas ^0.7.6 of Solidity in [GraphProxy.sol]
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L3

```
Found usage of floating pragmas ^0.7.6 of Solidity in [GraphProxyAdmin.sol]
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L3

```
Found usage of floating pragmas ^0.7.6 of Solidity in [GraphProxyStorage.sol]
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyStorage.sol#L3

```
Found usage of floating pragmas ^0.7.6 of Solidity in [GraphUpgradeable.sol]
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphUpgradeable.sol#L3

```
Found usage of floating pragmas ^0.7.6 of Solidity in [IGraphProxy.sol]
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/IGraphProxy.sol#L3






### <a href="#Summary">[NC&#x2011;7]</a><a name="NC&#x2011;7"> Use of Block.Timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.
References: SWC ID: 116

#### <ins>Proof Of Concept</ins>


```
lastPausePartialTime = block.timestamp;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Pausable.sol#L32

```
lastPauseTime = block.timestamp;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Pausable.sol#L46

```
require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L95



#### <ins>Recommended Mitigation Steps</ins>
Block timestamps should not be used for entropy or generating random numbers—i.e., they should not be the deciding factor (either directly or through some derivation) for winning a game or changing an important state.

Time-sensitive logic is sometimes required; e.g., for unlocking contracts (time-locking), completing an ICO after a few weeks, or enforcing expiry dates. It is sometimes recommended to use block.number and an average block time to estimate times; with a 10 second block time, 1 week equates to approximately, 60480 blocks. Thus, specifying a block number at which to change a contract state can be more secure, as miners are unable to easily manipulate the block number.




### <a href="#Summary">[NC&#x2011;8]</a><a name="NC&#x2011;8"> Non-usage of specific imports

The current form of relative path import is not recommended for use because it can unpredictably pollute the namespace.
Instead, the Solidity docs recommend specifying imported symbols explicitly.
https://docs.soliditylang.org/en/v0.8.15/layout-of-source-files.html#importing-other-source-files

#### <ins>Proof Of Concept</ins>


```
import "./IGraphCurationToken.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/ICuration.sol#L5

```
import "../upgrades/GraphUpgradeable.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/BridgeEscrow.sol#L5

```
import "../governance/Managed.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/BridgeEscrow.sol#L6

```
import "../token/IGraphToken.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/BridgeEscrow.sol#L7

```
import "../upgrades/GraphUpgradeable.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/GraphTokenGateway.sol#L5

```
import "../arbitrum/ITokenGateway.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/GraphTokenGateway.sol#L6

```
import "../governance/Pausable.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/GraphTokenGateway.sol#L7

```
import "../governance/Managed.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/GraphTokenGateway.sol#L8

```
import "../arbitrum/L1ArbitrumMessenger.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L9

```
import "./GraphTokenGateway.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L10

```
import "./IController.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L5

```
import "../curation/ICuration.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L7

```
import "../epochs/IEpochManager.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L8

```
import "../rewards/IRewardsManager.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L9

```
import "../staking/IStaking.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L10

```
import "../token/IGraphToken.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L11

```
import "../arbitrum/ITokenGateway.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L12

```
import "../../arbitrum/L2ArbitrumMessenger.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L9

```
import "../../arbitrum/AddressAliasHelper.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L10

```
import "../../gateway/GraphTokenGateway.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L11

```
import "../../gateway/ICallhookReceiver.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L12

```
import "../token/L2GraphToken.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L13

```
import "../../upgrades/GraphUpgradeable.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L9

```
import "../../governance/Governed.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L10

```
import "./GraphTokenUpgradeable.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol#L7

```
import "../../arbitrum/IArbToken.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol#L8

```
import "./IStakingData.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L6

```
import "./GraphProxyStorage.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L7

```
import "../governance/Governed.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L5

```
import "./IGraphProxy.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L7

```
import "./GraphUpgradeable.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L8

```
import "./IGraphProxy.sol";
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphUpgradeable.sol#L5




#### <ins>Recommended Mitigation Steps</ins>

Use specific imports syntax per solidity docs recommendation.



### <a href="#Summary">[NC&#x2011;9]</a><a name="NC&#x2011;9"> Expressions for constant values such as a call to `keccak256()`, should use `immutable` rather than `constant`

While it doesn't save any gas because the compiler knows that developers often make this mistake, it's still best to use the right tool for the task at hand. There is a difference between `constant` variables and `immutable` variables, and they should each be used in their appropriate contexts. `constants` should be used for literal values written into the code, and `immutable` variables should be used for expressions, or values calculated in, or passed into the constructor.

#### <ins>Proof Of Concept</ins>

```
bytes32 private constant DOMAIN_TYPE_HASH =
        keccak256(
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#0

```
bytes32 private constant DOMAIN_NAME_HASH = keccak256("Graph Token");
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L38

```
bytes32 private constant DOMAIN_VERSION_HASH = keccak256("0");
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L39

```
bytes32 private constant DOMAIN_SALT =
        0xe33842a7acd1d5a1d28f25a931703e5605152dc48d64dc4716efdae1f5659591; // Randomly generated salt
    bytes32 private constant PERMIT_TYPEHASH =
        keccak256(
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#0





### <a href="#Summary">[NC&#x2011;10]</a><a name="NC&#x2011;10"> Use `bytes.concat()`

Solidity version 0.8.4 introduces `bytes.concat()` (vs `abi.encodePacked(<bytes>,<bytes>)`)

#### <ins>Proof Of Concept</ins>


```
        bytes32 nameHash = keccak256(abi.encodePacked(_name)
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L174

```
            abi.encodePacked(
                "\x19\x01",
                DOMAIN_SEPARATOR,
                keccak256(
                    abi.encode(PERMIT_TYPEHASH, _owner, _spender, _value, nonces[_owner], _deadline)
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#0




#### <ins>Recommended Mitigation Steps</ins>

Use `bytes.concat()` and upgrade to at least Solidity version 0.8.4 if required. 



