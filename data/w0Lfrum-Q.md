# QA Report

### Summary

The overall code is well-commented. Unit tests are provided. The logic is split into the corresponding files. The logic is clear when referring to the docs/information given in the code4rena contest page.
There can be improvements in the overall code by updating the compiler to the latest version (>0.8). 
Using an old compiler version compells the use of outdated OpenZeppelin libraries which do not include the latest updates.
Moeover, using an outdated compiler version can be problematic especially if there are publicly disclosed bugs and issues that affect the current(outdated) compiler version.

Advantages of using one of the latest compiler versions include :

+ Abicoder v2 is activated by default. Hence it would not be required to explicitly specify `pragma abicoder v2`

+ Checks for overflow/underflow would be activated by default. There would be no requirement for importing OpenZeppelin SafeMath Libraries

### Recommendation :

It is recommended to use the latest compiler version (i.e. 0.8.17)