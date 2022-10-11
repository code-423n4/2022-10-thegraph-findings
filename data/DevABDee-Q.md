## 1. Older Compiler Version Used, unnecessary use of SafeMath. 
Use at least 0.8.x compiler version. Since Solidity 0.8, the overflow/underflow check is implemented on the language level - it adds the validation to the bytecode during compilation. You don't need the SafeMath library for Solidity 0.8+.

## 2. Floating `Pragma` is a bad practice.
Floating pragmas are considered a bad practice.
It is always recommended that pragma should be fixed (not floating) to the version that you are intending to deploy your contracts with.
