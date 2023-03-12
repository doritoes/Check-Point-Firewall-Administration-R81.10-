# Chapter 6

## Configuring Gaia using CLISH pp. 216-218
1. Yes, the step 2 creates access via the account secadmin, which doesn't exist on CPCM1. This is by design.
2. Missing default gateway route
    - `set static-route default nexthop gateway address 200.100.0.254 priority 1 on`
    - You should now be able to ping 8.8.8.8 from CPCM2
