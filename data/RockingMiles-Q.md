## Missing fee parameter validation


Some fee parameters of functions are not checked for invalid values. Validate the parameters:
        
        
### Code instances:

        Rebates.addToPool (_indexerFees)
        Staking._setDelegationParameters (_queryFeeCut)
        Cobbs.cobbDouglas (fees)
        Cobbs.cobbDouglas (totalFees)
        Staking.setDelegationParameters (_queryFeeCut)
        Rebates.redeem (_indexerFees)



## safeApprove of openZeppelin is deprecated


You use safeApprove of openZeppelin although it's deprecated.
(see https://github.com/OpenZeppelin/openzeppelin-contracts/blob/566a774222707e424896c0c390a84dc3c13bdcb2/contracts/token/ERC20/utils/SafeERC20.sol#L38)
You should change it to increase/decrease Allowance as OpenZeppilin says.
        
### Code instances:

        Deprecated safeApprove in BridgeEscrow.sol line 28: graphToken().approve(_spender, type(uint256).max);
        Deprecated safeApprove in GNS.sol line 173: graphToken().approve(address(curation()), MAX_UINT256);
        Deprecated safeApprove in AllocationExchange.sol line 77: graphToken.approve(address(staking), MAX_UINT256);



## Require with empty message

The following requires are with empty messages. 
This is very important to add a message for any require. So the user has enough information to know the reason of failure.
### Code instances:

        Solidity file: GraphProxyAdmin.sol, In line 47 with Empty Require message.
        Solidity file: BancorFormula.sol, In line 416 with Empty Require message.
        Solidity file: BancorFormula.sol, In line 284 with Empty Require message.
        Solidity file: GraphProxyAdmin.sol, In line 59 with Empty Require message.
        Solidity file: BancorFormula.sol, In line 366 with Empty Require message.
        Solidity file: BancorFormula.sol, In line 327 with Empty Require message.
        Solidity file: GraphProxy.sol, In line 133 with Empty Require message.
        Solidity file: GraphProxyAdmin.sol, In line 34 with Empty Require message.
        Solidity file: BancorFormula.sol, In line 509 with Empty Require message.



## Require with not comprehensive message

The following requires has a non comprehensive messages. 
This is very important to add a comprehensive message for any require. Such that the user has enough 
information to know the reason of failure: 

### Code instances:

        Solidity file: Staking.sol, In line 1185 with Require message: <epochs
        Solidity file: Staking.sol, In line 428 with Require message: <cooldown
        Solidity file: GraphGovernance.sol, In line 72 with Require message: proposed




## Not verified input


external / public functions parameters should be validated to make sure the address is not 0.
Otherwise if not given the right input it can mistakenly lead to loss of user funds.    

### Code instances:

        Controller.sol.updateController _controller
        DisputeManager.sol.createIndexingDispute _allocationID
        GraphToken.sol.mint _to
        Curation.sol.initialize _controller
        EthereumDIDRegistry.sol.revokeAttributeSigned identity
        GNS.sol.initialize _bondingCurve
        Staking.sol.setAssetHolder _assetHolder
        AllocationExchange.sol.withdraw _to



## Solidity compiler versions mismatch


The project is compiled with different versions of solidity, which is not recommended because it can lead to undefined behaviors.
        


## Use safe math for solidity version <8

You should use safe math for solidity version <8 since there is no default over/under flow check it suchversions of solidity.
### Code instances:

        The contract WithdrawHelper.sol doesn't use safe math and is of solidity version < 8
        The contract GraphGovernanceStorage.sol doesn't use safe math and is of solidity version < 8
        The contract SubgraphNFT.sol doesn't use safe math and is of solidity version < 8
        The contract Managed.sol doesn't use safe math and is of solidity version < 8
        The contract AllocationExchange.sol doesn't use safe math and is of solidity version < 8


## Not verified owner


        owner param should be validated to make sure the owner address is not address(0).
        Otherwise if not given the right input all only owner accessible functions will be unaccessible.
        
        
### Code instances:

        EthereumDIDRegistry.sol.changeOwner newOwner
        L2GraphToken.sol.initialize _owner
        EthereumDIDRegistry.sol.changeOwnerSigned newOwner
        GraphCurationToken.sol.initialize _owner



## Init frontrun

Most contracts use an init pattern (instead of a constructor) to initialize contract parameters. Unless these are enforced to be atomic with contact deployment via deployment script or factory contracts, they are susceptible to front-running race conditions where an attacker/griefer can front-run (cannot access control because admin roles are not initialized) to initially with their own (malicious) parameters upon detecting (if an event is emitted) which the contract deployer has to redeploy wasting gas and risking other transactions from interacting with the attacker-initialized contract.

Many init functions do not have an explicit event emission which makes monitoring such scenarios harder. All of them have re-init checks; while many are explicit some (those in auction contracts) have implicit reinit checks in initAccessControls() which is better if converted to an explicit check in the main init function itself.
(details credit to: https://github.com/code-423n4/2021-09-sushimiso-findings/issues/64)
The vulnerable initialization functions in the codebase are: 

### Code instance:

        GraphCurationToken.sol, initialize, 26



## Named return issue

Users can mistakenly think that the return value is the named return, but it is actually the actualreturn statement that comes after. To know that the user needs to read the code and is confusing.
Furthermore, removing either the actual return or the named return will save gas. 

### Code instances:

        LibFixedMath.sol, _mul
        LibFixedMath.sol, toInteger
        LibFixedMath.sol, exp
        Cobbs.sol, cobbDouglas
        LibFixedMath.sol, ln



## Two Steps Verification before Transferring Ownership

The following contracts have a function that allows them an admin to change it to a different address. If the admin accidentally uses an invalid address for which they do not have the private key, then the system gets locked.
It is important to have two steps admin change where the first is announcing a pending new admin and the new address should then claim its ownership. 
A similar issue was reported in a previous contest and was assigned a severity of medium: [code-423n4/2021-06-realitycards-findings#105](https://github.com/code-423n4/2021-06-realitycards-findings/issues/105) 

### Code instances:

        GraphProxyStorage.sol
        GNS.sol
        GraphProxy.sol
        IGNS.sol
        IGraphProxy.sol



## Missing non reentrancy modifier

The following functions are missing reentrancy modifier although some other pulbic/external functions does use reentrancy modifer.
Even though I did not find a way to exploit it, it seems like those functions should have the nonReentrant modifier as the other functions have it as well..

### Code instance:

        L2GraphTokenGateway.sol, outboundTransfer is missing a reentrancy modifier



## Assert instead require to validate user inputs


        From solidity docs: Properly functioning code should never reach a failing assert statement; if this happens there is a bug in your contract which you should fix.
        With assert the user pays the gas and with require it doesn't. The ETH network gas isn't cheap and users can see it as a scam.
        
### Code instances:

        GraphProxy.sol : reachable assert in line 46
        GraphProxy.sol : reachable assert in line 50
        GraphProxy.sol : reachable assert in line 47



## In the following public update functions no value is returned

In the following functions no value is returned, due to which by default value of return will be 0. 
We assumed that after the update you return the latest new value. 
(similar issue here: https://github.com/code-423n4/2021-10-badgerdao-findings/issues/85). 

### Code instances:

        GNS.sol, updateSubgraphMetadata
        GraphGovernance.sol, updateProposal



## Never used parameters

Those are functions and parameters pairs that the function doesn't use the parameter. In case those functions are external/public this is even worst since the user is required to put value that never used and can misslead him and waste its time. 

### Code instance:

        L2ArbitrumMessenger.sol: function sendTxToL1 parameter _l1CallValue isn't used. (sendTxToL1 is internal)



## Check transfer receiver is not 0 to avoid burned money


Transferring tokens to the zero address is usually prohibited to accidentally avoid "burning" tokens by sending them to an unrecoverable zero address.


### Code instances:

        https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L244
        https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L276


## Add a timelock

To give more trust to users: functions that set key/critical variables should be put behind a timelock.

### Code instances:

        https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Controller.sol#L98
        https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L121
        https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/Curation.sol#L124


## Must approve 0 first


Some tokens (like USDT) do not work when changing the allowance from an existing non-zero allowance value.
They must first be approved by zero and then the actual allowance must be approved.

### Code instances:

    approve without approving 0 first AllocationExchange.sol, 77,         graphToken.approve(address(staking), MAX_UINT256);
    approve without approving 0 first GNS.sol, 173,         graphToken().approve(address(curation()), MAX_UINT256);
    approve without approving 0 first BridgeEscrow.sol, 28,         graphToken().approve(_spender, type(uint256).max);
    



## Div by 0


Division by 0 can lead to accidentally revert,
(An example of a similar issue - https://github.com/code-423n4/2021-10-defiprotocol-findings/issues/84)

### Code instances:

        https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/bancor/BancorFormula.sol#L295 _amount might be 0
        https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/bancor/BancorFormula.sol#L257 result might be 0



## approve return value is ignored

Some tokens don't correctly implement the EIP20 standard and their approve function returns void instead of a success boolean. 
Calling these functions with the correct EIP20 function signatures will always revert.
Tokens that don't correctly implement the latest EIP20 spec, like USDT, will be unusable in the mentioned contracts as they revert the transaction because of the missing return value.
We recommend using OpenZeppelin’s SafeERC20 versions with the safeApprove function that handle the return value check as well as non-standard-compliant tokens.
The list of occurrences in format (solidity file, line number, actual line)

### Code instances:
    
    AllocationExchange.sol, 77,         graphToken.approve(address(staking), MAX_UINT256);
    BridgeEscrow.sol, 28,         graphToken().approve(_spender, type(uint256).max);



