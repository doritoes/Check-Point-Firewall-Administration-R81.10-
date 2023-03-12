# Chapter 3

## Installing Oracle VirtualBox, pp. 77-88
Installing VirtualBox with the default Python 2.x setting causes an error to be displayed.

Simply accept the error message and continue, ignoring the Python 2 dependencies. The alternative is to de-select the Python 2.x settings

## Installing VoyOS Router, pp. 86-96

1. Kernel Panic booting the VoyOS router
    - Set the number of CPUs to 2, not 1
2. Missing step page 95 step 4
    - add step 4 E. exit
    - before you show interfaces, you need to `exit` from configuration mode
3. Missing command and line wrapping issues page 95, step 8
    - before entering the block of code, enter the `configure` command
    - line wrapping breaks up the code

```
set interfaces ethernet eth0 description 'OUTSIDE'
set interfaces ethernet eth1 address '200.100.0.254/24'
set interfaces ethernet eth1 description 'Net_200.100.0.0'
set interfaces ethernet eth2 address '200.200.0.254/24'
set interfaces ethernet eth2 description 'Net_200.200.0.0'
set nat source rule 100 outbound-interface 'eth0'
set nat source rule 100 source address '200.100.0.0/24'
set nat source rule 100 translation address 'masquerade'
set nat source rule 200 outbound-interface 'eth0'
set nat source rule 200 source address '200.100.0.0/24'
set nat source rule 200 translation address 'masquerade'
set system host-name 'router'
set system name-server '9.9.9.9
```
4. After step 13, confirm the default gateway static route was automatically added after configuring eth0 for dhcp
``` show ip route ```
If missing, you can configure it like this:
```
configure
set protocols static route 0.0.0.0/0 dhcp-interface eth0
commit
save
exit
```
When this disappears, rebooting the router restores the route.
