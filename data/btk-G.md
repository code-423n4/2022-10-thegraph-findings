The simplest thing that you can do to reduce the code size, is to wrap any code inside a function modifier into a function.

contract L2GraphToken:

Before:

modifier onlyGateway() {
        require(msg.sender == gateway, "NOT_GATEWAY");
        _;
    }

After:

function _onlyGateway() private view {
        require(msg.sender == gateway, "NOT_GATEWAY");
}

modifier onlyGateway() {
        _onlyGateway;
        _;
    }

contract L2GraphTokenGateway:

Before:

modifier onlyL1Counterpart() {
        require(
            msg.sender == AddressAliasHelper.applyL1ToL2Alias(l1Counterpart),
            "ONLY_COUNTERPART_GATEWAY"
        );
        _;
    }

After:

function _onlyL1Counterpart() private view {
        require(msg.sender == AddressAliasHelper.applyL1ToL2Alias(l1Counterpart),"ONLY_COUNTERPART_GATEWAY");
}

modifier onlyL1Counterpart() {
        _onlyL1Counterpart;
        _;
    }