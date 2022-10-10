## In the file contracts/gateway/BridgeEscrow.sol: 
`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

[!]DO NOT USE SOLIDITY VERSION LESS THAN 0.8.0 FOR SAFE CHECK

[!]USE A MORE RECENT VERSION OF SOLIDITY.

AVOID CONTRACT EXISTENCE CHECKS BY USING SOLIDITY VERSION 0.8.10 OR LATER
Prior to 0.8.10 the compiler inserted extra code, including `EXTCODESIZE` (**100 gas**), to check for contract existence for external calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value

## In the file contracts/upgrades/GraphUpgradeable.sol: 
`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

[!]DO NOT USE SOLIDITY VERSION LESS THAN 0.8.0 FOR SAFE CHECK

[!]USE A MORE RECENT VERSION OF SOLIDITY.

AVOID CONTRACT EXISTENCE CHECKS BY USING SOLIDITY VERSION 0.8.10 OR LATER
Prior to 0.8.10 the compiler inserted extra code, including `EXTCODESIZE` (**100 gas**), to check for contract existence for external calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value

Line 24:        require(msg.sender == _proxy.admin(), "Caller must be the proxy admin");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 32:        require(msg.sender == _implementation(), "Caller must be the implementation");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 32:        require(msg.sender == _implementation(), "Caller must be the implementation");
REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS

## In the file contracts/governance/Governed.sol: 
`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

[!]DO NOT USE SOLIDITY VERSION LESS THAN 0.8.0 FOR SAFE CHECK

[!]USE A MORE RECENT VERSION OF SOLIDITY.

AVOID CONTRACT EXISTENCE CHECKS BY USING SOLIDITY VERSION 0.8.10 OR LATER
Prior to 0.8.10 the compiler inserted extra code, including `EXTCODESIZE` (**100 gas**), to check for contract existence for external calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value

STORAGE VARIABLES LOADED TWOCE SHOULD GET CACHED
In function `transferOwnership`:
`pendingGovernor` should be cached to save gas

Line 24:        require(msg.sender == governor, "Only Governor can call");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 41:        require(_newGovernor != address(0), "Governor must be set");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 54:        require(
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 46:        emit NewPendingOwnership(oldPendingGovernor, pendingGovernor);
AVOID EMITTING A STORAGE VARIABLE WHEN A MEMORY VALUE IS AVAILABLE

Line 65:        emit NewOwnership(oldGovernor, governor);
AVOID EMITTING A STORAGE VARIABLE WHEN A MEMORY VALUE IS AVAILABLE

Line 66:        emit NewPendingOwnership(oldPendingGovernor, pendingGovernor);
AVOID EMITTING A STORAGE VARIABLE WHEN A MEMORY VALUE IS AVAILABLE

## In the file contracts/governance/Pausable.sol: 
`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

[!]DO NOT USE SOLIDITY VERSION LESS THAN 0.8.0 FOR SAFE CHECK

[!]USE A MORE RECENT VERSION OF SOLIDITY.

AVOID CONTRACT EXISTENCE CHECKS BY USING SOLIDITY VERSION 0.8.10 OR LATER
Prior to 0.8.10 the compiler inserted extra code, including `EXTCODESIZE` (**100 gas**), to check for contract existence for external calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value

STORAGE VARIABLES LOADED TWOCE SHOULD GET CACHED
In function `_setPartialPaused`:
`_partialPaused` should be cached to save gas

STORAGE VARIABLES LOADED TWOCE SHOULD GET CACHED
In function `_setPaused`:
`_paused` should be cached to save gas

Line 8:    bool internal _partialPaused;
USING `BOOL`S FOR STORAGE INCURS OVERHEAD: use uint256(1) uint256(2) instead.

Line 10:    bool internal _paused;
USING `BOOL`S FOR STORAGE INCURS OVERHEAD: use uint256(1) uint256(2) instead.

Line 34:        emit PartialPauseChanged(_partialPaused);
AVOID EMITTING A STORAGE VARIABLE WHEN A MEMORY VALUE IS AVAILABLE

Line 48:        emit PauseChanged(_paused);
AVOID EMITTING A STORAGE VARIABLE WHEN A MEMORY VALUE IS AVAILABLE

Line 58:        emit NewPauseGuardian(oldPauseGuardian, pauseGuardian);
AVOID EMITTING A STORAGE VARIABLE WHEN A MEMORY VALUE IS AVAILABLE

## In the file contracts/l2/token/L2GraphToken.sol: 
`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

[!]DO NOT USE SOLIDITY VERSION LESS THAN 0.8.0 FOR SAFE CHECK

[!]USE A MORE RECENT VERSION OF SOLIDITY.

AVOID CONTRACT EXISTENCE CHECKS BY USING SOLIDITY VERSION 0.8.10 OR LATER
Prior to 0.8.10 the compiler inserted extra code, including `EXTCODESIZE` (**100 gas**), to check for contract existence for external calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value

Line 36:        require(msg.sender == gateway, "NOT_GATEWAY");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 49:        require(_owner != address(0), "Owner must be set");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 60:        require(_gw != address(0), "INVALID_GATEWAY");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 70:        require(_addr != address(0), "INVALID_L1_ADDRESS");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 62:        emit GatewaySet(gateway);
AVOID EMITTING A STORAGE VARIABLE WHEN A MEMORY VALUE IS AVAILABLE

## In the file contracts/upgrades/GraphProxyAdmin.sol: 
`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

[!]DO NOT USE SOLIDITY VERSION LESS THAN 0.8.0 FOR SAFE CHECK

[!]USE A MORE RECENT VERSION OF SOLIDITY.

AVOID CONTRACT EXISTENCE CHECKS BY USING SOLIDITY VERSION 0.8.10 OR LATER
Prior to 0.8.10 the compiler inserted extra code, including `EXTCODESIZE` (**100 gas**), to check for contract existence for external calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value

Line 34:        require(success);
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 47:        require(success);
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 59:        require(success);
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

## In the file contracts/upgrades/GraphProxyStorage.sol: 
`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

[!]DO NOT USE SOLIDITY VERSION LESS THAN 0.8.0 FOR SAFE CHECK

[!]USE A MORE RECENT VERSION OF SOLIDITY.

AVOID CONTRACT EXISTENCE CHECKS BY USING SOLIDITY VERSION 0.8.10 OR LATER
Prior to 0.8.10 the compiler inserted extra code, including `EXTCODESIZE` (**100 gas**), to check for contract existence for external calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value

Line 62:        require(msg.sender == _admin(), "Caller must be admin");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

## In the file contracts/upgrades/GraphProxy.sol: 
`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

[!]DO NOT USE SOLIDITY VERSION LESS THAN 0.8.0 FOR SAFE CHECK

[!]USE A MORE RECENT VERSION OF SOLIDITY.

AVOID CONTRACT EXISTENCE CHECKS BY USING SOLIDITY VERSION 0.8.10 OR LATER
Prior to 0.8.10 the compiler inserted extra code, including `EXTCODESIZE` (**100 gas**), to check for contract existence for external calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value

Line 47:        assert(ADMIN_SLOT == bytes32(uint256(keccak256("eip1967.proxy.admin")) - 1));
ASSERT() USED! ASSERT USES UP ALL REMAINING GAS WHEN REVERT!

Line 48:        assert(
ASSERT() USED! ASSERT USES UP ALL REMAINING GAS WHEN REVERT!

Line 51:        assert(
ASSERT() USED! ASSERT USES UP ALL REMAINING GAS WHEN REVERT!

Line 105:        require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 105:        require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS

Line 133:        require(success);
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 141:        require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 141:        require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS

Line 142-145:                require(
            _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
            "Caller must be the pending implementation"
        );
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.
REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS

Line 157:        require(msg.sender != _admin(), "Cannot fallback to proxy target");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 179:                revert(ptr, size)
REVERT() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

## In the file contracts/governance/Managed.sol: 
`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

[!]DO NOT USE SOLIDITY VERSION LESS THAN 0.8.0 FOR SAFE CHECK

[!]USE A MORE RECENT VERSION OF SOLIDITY.

AVOID CONTRACT EXISTENCE CHECKS BY USING SOLIDITY VERSION 0.8.10 OR LATER
Prior to 0.8.10 the compiler inserted extra code, including `EXTCODESIZE` (**100 gas**), to check for contract existence for external calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value

Line 44:        require(!controller.paused(), "Paused");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 45:        require(!controller.partialPaused(), "Partial-paused");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 49:        require(!controller.paused(), "Paused");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 53:        require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 53:        require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS

Line 57:        require(msg.sender == address(controller), "Caller must be Controller");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 104:        require(_controller != address(0), "Controller must be set");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

## In the file contracts/l2/token/GraphTokenUpgradeable.sol: 
`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

[!]DO NOT USE SOLIDITY VERSION LESS THAN 0.8.0 FOR SAFE CHECK

[!]USE A MORE RECENT VERSION OF SOLIDITY.

AVOID CONTRACT EXISTENCE CHECKS BY USING SOLIDITY VERSION 0.8.10 OR LATER
Prior to 0.8.10 the compiler inserted extra code, including `EXTCODESIZE` (**100 gas**), to check for contract existence for external calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value

STORAGE VARIABLES LOADED TWOCE SHOULD GET CACHED
In function `permit`:
`nonces[_owner]` should be cached to save gas

Line 60:        require(isMinter(msg.sender), "Only minter can call");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 94:        require(_owner == recoveredAddress, "GRT: invalid permit");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 95:        require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 95:        require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
REQUIRE() OR REVERT() STATEMENTS THAT CHECK INPUT ARGUMENTS SHOULD BE AT THE TOP OF THE FUNCTION

Line 106:        require(_account != address(0), "INVALID_MINTER");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 115:        require(_minters[_account], "NOT_A_MINTER");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 123:        require(_minters[msg.sender], "NOT_A_MINTER");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 51:    mapping(address => bool) private _minters;
USING `BOOL`S FOR STORAGE INCURS OVERHEAD: use uint256(1) uint256(2) instead.

Line 79:        uint8 _v,
USAGE OF `UINTS`/`INTS` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD: When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

## In the file contracts/l2/gateway/L2GraphTokenGateway.sol: 
`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

[!]DO NOT USE SOLIDITY VERSION LESS THAN 0.8.0 FOR SAFE CHECK

[!]USE A MORE RECENT VERSION OF SOLIDITY.

AVOID CONTRACT EXISTENCE CHECKS BY USING SOLIDITY VERSION 0.8.10 OR LATER
Prior to 0.8.10 the compiler inserted extra code, including `EXTCODESIZE` (**100 gas**), to check for contract existence for external calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value

STORAGE VARIABLES LOADED TWOCE SHOULD GET CACHED
In function `outboundTransfer`:
`l1GRT` should be cached to save gas

STORAGE VARIABLES LOADED TWOCE SHOULD GET CACHED
In function `finalizeInboundTransfer`:
`l1GRT` should be cached to save gas

Line 69:        require(
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 98:        require(_l2Router != address(0), "INVALID_L2_ROUTER");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 108:        require(_l1GRT != address(0), "INVALID_L1_GRT");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 118:        require(_l1Counterpart != address(0), "INVALID_L1_COUNTERPART");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 145:        require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 146:        require(_amount > 0, "INVALID_ZERO_AMOUNT");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 146:        require(_amount > 0, "INVALID_ZERO_AMOUNT");
USING `> 0` COSTS MORE GAS THAN `!= 0` WHEN USED ON A `UINT` IN A `REQUIRE()` STATEMENT

Line 147:        require(msg.value == 0, "INVALID_NONZERO_VALUE");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 148:        require(_to != address(0), "INVALID_DESTINATION");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 153:        require(outboundCalldata.extraData.length == 0, "CALL_HOOK_DATA_NOT_ALLOWED");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 233:        require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 234:        require(msg.value == 0, "INVALID_NONZERO_VALUE");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

## In the file contracts/gateway/L1GraphTokenGateway.sol: 
`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

[!]DO NOT USE SOLIDITY VERSION LESS THAN 0.8.0 FOR SAFE CHECK

[!]USE A MORE RECENT VERSION OF SOLIDITY.

AVOID CONTRACT EXISTENCE CHECKS BY USING SOLIDITY VERSION 0.8.10 OR LATER
Prior to 0.8.10 the compiler inserted extra code, including `EXTCODESIZE` (**100 gas**), to check for contract existence for external calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value

STORAGE VARIABLES LOADED TWOCE SHOULD GET CACHED
In function `removeFromCallhookWhitelist`:
`callhookWhitelist[_notWhitelisted]` should be cached to save gas

STORAGE VARIABLES LOADED TWOCE SHOULD GET CACHED
In function `finalizeInboundTransfer`:
`escrow` should be cached to save gas

Line 74:        require(inbox != address(0), "INBOX_NOT_SET");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 78:        require(msg.sender == address(bridge), "NOT_FROM_BRIDGE");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 82:        require(l2ToL1Sender == l2Counterpart, "ONLY_COUNTERPART_GATEWAY");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 110:        require(_inbox != address(0), "INVALID_INBOX");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 111:        require(_l1Router != address(0), "INVALID_L1_ROUTER");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 122:        require(_l2GRT != address(0), "INVALID_L2_GRT");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 132:        require(_l2Counterpart != address(0), "INVALID_L2_COUNTERPART");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 142:        require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 142:        require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
SPLITTING `REQUIRE()` STATEMENTS THAT USE `&&` SAVES GAS

Line 153:        require(_newWhitelisted != address(0), "INVALID_ADDRESS");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 154:        require(!callhookWhitelist[_newWhitelisted], "ALREADY_WHITELISTED");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 165:        require(_notWhitelisted != address(0), "INVALID_ADDRESS");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 166:        require(callhookWhitelist[_notWhitelisted], "NOT_WHITELISTED");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 200:        require(_l1Token == address(token), "TOKEN_NOT_GRT");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 201:        require(_amount > 0, "INVALID_ZERO_AMOUNT");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 201:        require(_amount > 0, "INVALID_ZERO_AMOUNT");
USING `> 0` COSTS MORE GAS THAN `!= 0` WHEN USED ON A `UINT` IN A `REQUIRE()` STATEMENT

Line 202:        require(_to != address(0), "INVALID_DESTINATION");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 202:        require(_to != address(0), "INVALID_DESTINATION");
REQUIRE() OR REVERT() STATEMENTS THAT CHECK INPUT ARGUMENTS SHOULD BE AT THE TOP OF THE FUNCTION

Line 213:                require(
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 217:                require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 217:                require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
USING `> 0` COSTS MORE GAS THAN `!= 0` WHEN USED ON A `UINT` IN A `REQUIRE()` STATEMENT

Line 224:                    require(msg.value >= expectedEth, "WRONG_ETH_VALUE");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 271:        require(_l1Token == address(token), "TOKEN_NOT_GRT");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 275:        require(_amount <= escrowBalance, "BRIDGE_OUT_OF_FUNDS");
REQUIRE() USED! USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE()/ASSERT() STRINGS TO SAVE GAS.

Line 35:    mapping(address => bool) public callhookWhitelist;
USING `BOOL`S FOR STORAGE INCURS OVERHEAD: use uint256(1) uint256(2) instead.


