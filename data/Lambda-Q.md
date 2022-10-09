# DOMAIN_SALT not necessary
According to EIP-712, "Protocol designers only need to include the fields that make sense for their signing domain. Unused fields are left out of the struct type." and it is mentioned that the salt is "an disambiguating salt for the protocol. This can be used as a domain separator of last resort.".

Therefore, the salt is not necessary for The Graph (the protocol is already identified with the other fields) and could be left out.