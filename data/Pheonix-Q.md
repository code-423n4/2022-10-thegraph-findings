# **Use a more recent version of solidity**

Use a solidity version of at least 0.8.4 to get bytes.concat() instead of abi.encodePacked(,) Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(,) Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions

## Instances-

File: inscope\BridgeEscrow.sol  pragma solidity ^0.7.6; 

File: inscope\Governed.sol  pragma solidity ^0.7.6; 

File: inscope\GraphProxy.sol pragma solidity ^0.7.6; 

File: inscope\GraphProxyAdmin.sol pragma solidity ^0.7.6;  

File: inscope\GraphProxyStorage.sol pragma solidity ^0.7.6;  

File: inscope\GraphTokenGateway.sol pragma solidity ^0.7.6;

File: inscope\GraphTokenUpgradeable.sol pragma solidity ^0.7.6; 

File: inscope\GraphUpgradeable.sol pragma solidity ^0.7.6; 

File: inscope\ICallhookReceiver.sol pragma solidity ^0.7.6; 

File: inscope\IController.sol pragma solidity >=0.6.12 <0.8.0;

File: inscope\ICuration.sol pragma solidity ^0.7.6; 

File: inscope\IEpochManager.sol pragma solidity ^0.7.6; 

File: inscope\IGraphCurationToken.sol pragma solidity ^0.7.6; 

File: inscope\IGraphProxy.sol pragma solidity ^0.7.6; 

File: inscope\IGraphToken.sol pragma solidity ^0.7.6; 

File: inscope\IRewardsManager.sol pragma solidity ^0.7.6; 

File: inscope\IStaking.sol pragma solidity >=0.6.12 <0.8.0; 

File: inscope\IStakingData.sol pragma solidity >=0.6.12 <0.8.0; 

File: inscope\L1GraphTokenGateway.sol pragma solidity ^0.7.6; 

File: inscope\L2GraphToken.sol pragma solidity ^0.7.6; 

File: inscope\L2GraphTokenGateway.sol pragma solidity ^0.7.6; 

File: inscope\Managed.sol pragma solidity ^0.7.6; 

File: inscope\Pausable.sol pragma solidity ^0.7.6; 

# **Floating Solidity Pragma Version**

It’s best to use the same compiler version across all project files/team members. So having a fixed version pragma is a good practice. Most contracts use a floating pragma which would allow the patch number to be equal or higher than the specified patch number.

### Instances-

File: inscope\BridgeEscrow.sol  pragma solidity ^0.7.6; 

File: inscope\Governed.sol  pragma solidity ^0.7.6; 

File: inscope\GraphProxy.sol pragma solidity ^0.7.6; 

File: inscope\GraphProxyAdmin.sol pragma solidity ^0.7.6;  

File: inscope\GraphProxyStorage.sol pragma solidity ^0.7.6;  

File: inscope\GraphTokenGateway.sol pragma solidity ^0.7.6;

File: inscope\GraphTokenUpgradeable.sol pragma solidity ^0.7.6; 

File: inscope\GraphUpgradeable.sol pragma solidity ^0.7.6; 

File: inscope\ICallhookReceiver.sol pragma solidity ^0.7.6; 

File: inscope\IController.sol pragma solidity >=0.6.12 <0.8.0;

File: inscope\ICuration.sol pragma solidity ^0.7.6; 

File: inscope\IEpochManager.sol pragma solidity ^0.7.6; 

File: inscope\IGraphCurationToken.sol pragma solidity ^0.7.6; 

File: inscope\IGraphProxy.sol pragma solidity ^0.7.6; 

File: inscope\IGraphToken.sol pragma solidity ^0.7.6; 

File: inscope\IRewardsManager.sol pragma solidity ^0.7.6; 

File: inscope\IStaking.sol pragma solidity >=0.6.12 <0.8.0; 

File: inscope\IStakingData.sol pragma solidity >=0.6.12 <0.8.0; 

File: inscope\L1GraphTokenGateway.sol pragma solidity ^0.7.6; 

File: inscope\L2GraphToken.sol pragma solidity ^0.7.6; 

File: inscope\L2GraphTokenGateway.sol pragma solidity ^0.7.6; 

File: inscope\Managed.sol pragma solidity ^0.7.6; 

File: inscope\Pausable.sol pragma solidity ^0.7.6; 

# No need to use SafeMath library

Solidity 0.8.0 and higher comes with implicit overflow and underflow condtions . There is no need to use SafeMath library provided that the complier version has upgraded to version greater than/equal to 0.8..0

## Instances-

## File:GraphTokenUpgradeable.sol

7: import "@openzeppelin/contracts/math/SafeMath.sol";

## File:L1GraphTokenGateway.sol

7: import "@openzeppelin/contracts/math/SafeMath.sol";

## File: L2GraphToken.sol

5: import "@openzeppelin/contracts/math/SafeMath.sol";

## File:L2GraphTokenGateway.sol

7: import "@openzeppelin/contracts/math/SafeMath.sol";
