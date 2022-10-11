
## Impact
functions can be made view

## Proof of Concept
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L69
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L82
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L95

## Tools Used

## Recommended Mitigation Steps
as an example
```
function implementation() external view ifAdminOrPendingImpl returns (address) {}
```

## Impact
use pull model for proxy admin. If admin were set to wrong address by mistake, the impact can be severe

## Proof of Concept
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L69

## Tools Used

## Recommended Mitigation Steps
as an example
```
function implementation() external view ifAdminOrPendingImpl returns (address) {}
```


## Impact
protect setting admin to implementation

## Proof of Concept
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L115

## Tools Used

## Recommended Mitigation Steps
```
    function upgradeTo(address _newImplementation) external ifAdmin { 
        **require(_newImplementation != address(admin))**
        _setPendingImplementation(_newImplementation);
    }
```
    
## Impact
replace direct `_implementation().delegatecall(data);` call with _fallback(), which uses assembly thus saving gas

## Proof of Concept
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L132

## Tools Used

## Recommended Mitigation Steps

