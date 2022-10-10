## CHEAPER INPUT VALDIATIONS SHOULD COME BEFORE EXPENSIVE OPERATIONS
`require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
 require(_amount > 0, "INVALID_ZERO_AMOUNT");
 require(msg.value == 0, "INVALID_NONZERO_VALUE");
 require(_to != address(0), "INVALID_DESTINATION");`

it should be :
` require(_to != address(0), "INVALID_DESTINATION");
  require(msg.value == 0, "INVALID_NONZERO_VALUE");
  require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
  require(_amount > 0, "INVALID_ZERO_AMOUNT");`
