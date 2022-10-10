# 1. [G-1] For loops: ++i  cost less gas compare to i++

Instances include:

    File contracts/rewards/RewardsManager.sol, line 161:       for (uint256 i = 0; i < _subgraphDeploymentID.length; i++)

I suggest using `++i` instead of `i++` to increment the value of an uint variable.

# 2. [G-2] For loops: Cache the length of arrays in the loops to save gas

    File contracts/rewards/RewardsManager.sol, line 161:       for (uint256 i = 0; i < _subgraphDeploymentID.length; i++)

I suggest storing the array’s length in a variable before the for-loop, and use it instead.

# 3. [G-3] Variables: No need to explicitly initialize variables with default values

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

Instances include:

    File contracts/rewards/RewardsManager.sol, line 161:       for (uint256 i = 0; i < _subgraphDeploymentID.length; i++)

I suggest removing explicit initializations for default values.

# 4. [G-4] Variables: "Constant" expressions are expressions, not constants

Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a constant variable is recomputed each time that the variable is used, which wastes some gas.

If the variable was immutable instead: the calculation would only be done once at deploy time (in the constructor), and then the result would be saved and read directly at runtime rather than being recalculated.

Instances include:

    File contracts/l2/token/GraphTokenUpgradeable.sol, line 38:  bytes32 private constant DOMAIN_NAME_HASH = keccak256("Graph Token");
    File contracts/l2/token/GraphTokenUpgradeable.sol, line 39:  bytes32 private constant DOMAIN_VERSION_HASH = keccak256("0");
    File contracts/l2/token/GraphTokenUpgradeable.sol, line 42-45:  bytes32 private constant PERMIT_TYPEHASH =
        keccak256(
            "Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)"
        );

Change these expressions from `constant` to `immutable` and implement the calculation in the constructor, or hardcode these values in the constants and add a comment to say how the value was calculated.

# 6. [G-6] Comparisons: Boolean comparations

Comparing to a constant (`true` or `false`) is a bit more expensive than directly checking the returned value boolean. I suggest using `require (condition, ...)` instead of `require (condition == true, ...)` here:

    File contracts/gateway/L1GraphTokenGateway.sol, line 213:     require(
                    extraData.length == 0 || callhookWhitelist[msg.sender] == true,
                    "CALL_HOOK_DATA_NOT_ALLOWED"
                );

# 7. [G-7] Storage: Emitting storage values

The values emitted shouldn't be read from storage. The existing memory values should be used instead in here:

    File contracts/l2/token/L2GraphToken.sol, line 61-62:   
            gateway = _gw;
            emit GatewaySet(gateway); // I suggest: emit GatewaySet(_gw);
    