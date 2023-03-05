# Chapter 12

## Configuring a remote gateway and creating its policy pp. 467-474
1. Page 467 step 2 has the interfaces mixed up
    - clomevms.bat has eth0 being 172.16.16.0 and eth1 being 200.200.0.0
    - Use the corrected commands below

clomevms.bat
```
VBoxManage modifyvm "CPGW" --nic1 intnet
VBoxManage modifyvm "CPGW" --nictype1 82540EM
VBoxManage modifyvm "CPGW" --macaddress1 auto
VBoxManage modifyvm "CPGW" --cableconnected1 on
VBoxManage modifyvm "CPGW" --intnet1 Net_172.16.16.0
VBoxManage modifyvm "CPGW" --nic2 intnet
VBoxManage modifyvm "CPGW" --nictype2 82540EM
VBoxManage modifyvm "CPGW" --macaddress2 auto
VBoxManage modifyvm "CPGW" --cableconnected2 on
VBoxManage modifyvm "CPGW" --intnet2 Net_200.200.0.0
```

Corrected code for pages 467-468
```
set hostname CPGW
set interface eth0 ipv4-address 172.16.16.1 subnet-mast 255.255.255.0
set interface eth0 state on
set interface eth1 ipv4-address 200.200.0.1 subnet-mask 255.255.255.0
set interface eth1 state on
set static-route default nexthop gateway address 200.200.0.254 on
add allowed-client host ipv4-address 10.0.0.20
add allowed-client host ipv4-address 200.100.0.20
delete allowed-client host any-host
set expert-password
```
2. Page 468 step 3 has the wrong IP address for the router. Instead, the book has the user ping the firewall's own IP address
    - `ping 200.200.0.254`
3. Page 468 step 7 has the user to leave the Internet connection unconfigured. This is intended.
4. Page 450 step 5 fails
    - Logs show failed communications on CPCXL from CPSMS to CPGW_tmp on tcp ports 18191 and 18211
    - It's best that as part of page 468 step 1 to create a new rule 2 to allow CPSMS to CPGW_tmp for services
        - CPD (tcp/19191)
        - CPD_amon (tcp/18192)
        - FW1 (tcp/256)
        - FW1_ica_push (tcp/18211)
    - Later on page 469 step 1, also remove the temporary rule 2 that was created
5. Page 471 figure 12.5 is incorrect if it was cloned using clonevms.bat from the repo and the configuration fixed (see above) to work
    - eth0 topology is Internal
    - eth1 topology is External
6. Page 472 steps 9 and 10 likewise don't match clonevms.bat from the repo
    - step 9 should be double-click eth1, modify topology, ExternalZone
    - step 10 should be double-click eth0, modify topology, override, network defined by the interface IP and Net Mask, enable Anti-Spoofing
## Star community - To center or through the center to other satellites, to Internet and other VPN targets pp. 479-483
1. Page 480 step 4 states that Internet traffic from RightHost should be working; it does not work
    - Update the APCL_URLF_Parent rule by adding Net_172.16.16.0 to source column, and push LeftSide_S2S policy
2. Step 6 is mising publishing and installing the LeftSide_S2S policy
    - Publish all changes and intall LeftSide_S2S policy
4. Page 481 last paragraph says to "SSH into CPGW". This is not possible from SmartConsole since page 469 step 1 removes CPGW_tmp and nowhere does it state with what to replace it
    - In LeftSite_S2S policy rule one, add "CPGW" to the destination. Publish and install policy to allow SSH to CPGW
## Changing portals' URLs and renewing a gateway cluster certificate pp.483-486
1. Page 485 step 3 will trigger an 'Are you sure?' warning
    - Click Yes to continue.
## Creating local user templates, groups, users and access roles
1. Page 489 last paragraph instructs the user to create Net_192.168.254.0
    - missing step to configure NAT for this object 192.168.254.0/24
        - check automatic address translation rules
        - Hide behind the gateway
        - Install on gateway: CPGW
    - missing step to add this object 192.168.254.0/24 to group IntNets
## Configuring a gateway or cluster for remote access pp. 489-492
1. Page 491 step 7 provides instructions that leave the setting "MAC address for DHCP allocation" at the default; Figure 12.24 has it set to "Calculated per user name"
    - Leave at the default value "MAC address for DHCP allocation" did not seem to break anything
## Configuring access control policy rules for remote access pp. 497-499
1. Page 499 step 1 discusses replacing the "Int_Nets" object; this is actually the "Any" object since the parent rule already is limited to Int_Nets
    - See above instructions for adding Net_192.168.254.0 to IntNets in order for this to work
    - Follow the book's instructions, understanding instead of "Int_Nets" you will see "Any"
2. Step 4 says to install the policy; it does not warn the users of the warning message that is displayed
    - "You are about to override the current policy with "LeftSide_RA" for these gateways:" is expected
    - click Install
## Preparing remote client pp. 500-506
1. On page 500, under Preparing Remote client, the second paragraph has the user change DNS to 9.9.9.9 and test accessing websites.
    - If you have trouble with Internet connectivity
        - Be sure you followed instructions on page 487 under "Configuring a remote access IPSec VPN", where you remove both gatewas from the "Branches" community.  Both policies need to be installed.
        - On the active CPCXL membere CPCM1 run `vpn tu` to see if the old site to site (S2S) tunnel are still up, and clear them if they are
    - Modify the "Branches" community, VPN Routing, set to "To center only"
    - Publish changes, and push both policies (LeftSide_RA and RightSide)

2. On page 502, after step 9, the Endpoint Security Client interrupts configuration with the warning "The site's security certificate is not trusted!"
    - re-installing the CPCXL VPN Certificate then un-installing/re-installing the endpoint did not clear the warning
    - after viewing the details, select Trust and continue
3. On page 502 step 10 instructs choosing the login option Standard (Default), but this option does not appear. Figure 12.37 shows "Standard (Default)", which did appear.
    - The only option available is vpn (Default); step 11 shows a display name of Standard, but the default is still vpn
    - Click Next
4. Page 504 step 15 has the wrong username; the username is case sensitive and must be entered exactly as on page 488
    - User name is `Ra_user1`
