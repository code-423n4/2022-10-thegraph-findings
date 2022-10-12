# gas optimization

## Avoid extra call for revoking allowance

On [contracts/gateway/BridgeEscrow.sol/#L36-L39](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol/#L36-L39) instead of decreasing allowance and check user allowance, you could just do a `grt.approve(0)`

```diff
diff --git a/contracts/gateway/BridgeEscrow.sol b/contracts/gateway/BridgeEscrow.sol
index 605f13a..81b7a7c 100644
--- a/contracts/gateway/BridgeEscrow.sol
+++ b/contracts/gateway/BridgeEscrow.sol
@@ -34,7 +34,6 @@ contract BridgeEscrow is GraphUpgradeable, Managed {
      * @param _spender Address of the spender that will be revoked
      */
     function revokeAll(address _spender) external onlyGovernor {
-        IGraphToken grt = graphToken();
-        grt.decreaseAllowance(_spender, grt.allowance(address(this), _spender));
+        graphToken().approve(_spender, 0);
     }
 }
 ```

 ## Cache the keccak hash in a constant

 On:
 - https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L114
 - https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L122
 - https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L130
 - https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L138
 - https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L146
 - https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L154

 You could use a constant with the precalculated keccak.


## Unused SafeMath lib.

Remove safemathlib on; 
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol/#L24

And Safemath import
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol/#L7


## Use `+1` instead of safe math (its a safe operation)
On [contracts/l2/token/GraphTokenUpgradeable.sol/#L97](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol/#L97)
Its safe to do `+ 1` instead of `.add(1)`, `nonces[address]` is an uint256.
If you do so you could also remove the import fo the safemath libraries.

## Avoid calling `burnFrom` or `_mint` if there is nothing to burn/mint
on https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol/#L91
Before calling `burnFrom` check before that `_amount > 0`

Similar on https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol/#L81
Before calling `_mint` check before that `_amount > 0`