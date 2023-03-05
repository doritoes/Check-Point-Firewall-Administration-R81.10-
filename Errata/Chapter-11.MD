# Chapter 11

## Deploying the access control policy structure p. 428
1. The stealth rule showed only protects the physical IP address of the clutser member. The reference to Chapter 8 page 326 shows an ambiguous group named "CP_Gateways".
    - some administrators also block all traffic to the firewall cluster IP address while explicity allowing ping or perhaps traceroute just above it
2. There is a bug in SmartConsole where the access control policy is displayed as blank
    - close SmartConsole and re-open and the policy will display correctly
## Configuring DHCP on Windows Server p. 432
1. If the "Server Manager App" does not open automatically, user may feel lost installing DHCP and configuring a DHCP scope. Search the Start menu for Add Rules and Features doesn't return anything useful.
    - Start Server Manager
    - Add roles or features
        - next, next, select ADDDCDNS and click Next
        - check DHCP Server and click Add Features
        - click Next, Next, Next
        - click Install, wait for it to complete, click Close
        - Reboot
    - In Server Manager you will see the new DHCP line
        - Click DHCP, then right-click ADDCDNS and click DHCP Manager
    - Launch DHCP Manager
    - Expand the tree
    - Right-Click "IPv4" and select New Scope
        - Name: Left_Side_DHCP_Hosts
        - Start IP address: 10.10.10.51
        - End IP address: 10.10.10.60
        - Length 24 or subnet mask 255.255.255.0
        - No exclusions or delay
        - Lease duration: the default 8 days is a little long for our lab, try 8 hours
        - Yes, configure DHCP options: router 10.10.10.1, DNS 10.20.20.10, domain mycp.lab
        - No WINS servers
        - Activate the scope, Finish
3. See [resources online](https://www.technig.com/install-and-configure-dhcp-using-powershell-in-windows-server-2022/) if you are interested in scripting the enabling and configuration of DHCP
## Configuring rules for core services pp. 433-434
1. The NTP rule shown only allows access to the Windows time service. Back on page 193 NTP was configured for ntp.checkpoint.com on CPCM1
    - Optionally, modify CPSMS time settings; modify the NTP rule or split into two rules, create the domain object .ntp.checkpoint.com, and permit NTP access
    - this is a good time to go back and configure CPCM2 time settings too
## Configuring rules for privileged access pp. 434-435
1. Figure 11.7 shows the object "Router", but the objects created are actually "RouterLeft"and "RouterRight"
2. Figure 11.7 show an incorrect rule  name, should be something like "Block RDP non-admins Drop."
## Configuring DMZ access rules p. 436
1. User will need to power om DMZSRV for the first time. While the 1 vCPU maxxed out a lot, it is survivable.
2. Rule 17 has an incorrect rule name. It should be "Internal1 network to DMZ Accept"
## Missing policy push and connectivity testing p.436
1. Recommend after configuring DMZ access rules to:
    - create internet access rule under APCL & URL, Content Awareness Inline Layer
        - Name: APCL_URLF_Parent Rule
        - Source: Int_Nets
        - Destination: ExternalZone
        - Services: *Any
        - Action: Allow
        - Track: Log
    - push policy
    - power on LeftHost and browse to http://10.30.30.10 on DMZSRV
        - note that the DMZSRV really strugged with one (1) vCPU; better with two
    - test internet access to google.com from SmartConsole and LeftHost; if these tests fail:
        - check that the router VM is healthy and can ping to the internet (has default, route, etc.); reboot as needed
        - check that ADDCDNS DNS lookups are succeeding
## Configuring rules for access to updatable objects pp. 436-437
1. Importing updatable objects requires Internet access, which is not yet working
    - no firewall push (the first push is on page 444, without much explanation)
    - the action will fail, so the recommendation is to disable this incomplete rule for now
    - see workaround above, "Missing policy push and connectivity testing"; in our testing it still failed
    - See sk122636 for troubleshooting this issue
    - as a workaround, add the domains listed on page 231 for CPUSE
        - .updates.checkpoint.com
        - .updates.g01.checkpoint.com
        - .gwevents.checkpont.com
        - .gwevents.us.checkpont.com
        - .deploy.static.akamaitechnologies.com
## Configuring the HTTPS Inspection p. 444
1. Importing updatable objects requires Internet access, which is not yet working
    - no firewall push (the first push is at the bottom of this page without much explanation)
    - the action will fail, so the recommendation is to disable this incomplete rule for now
    - those is no log setting in the screen shot or in the instructions, so it is up to the user to decide to enable logging on the rule
2. The first time pushing policy is unceremoniously introduced as "Now is a good time to publish the pending changes and install the policy befor moving on to the next section"
    - recommending pushing policy must more frequently in building a lab, so the user can detect and fix issues early on
    - from the Standard policy you have created click the Install Policy button or click the main hamburger menu and click Install Policy
    - install both Access Controll and Threat Prevention Policies
    - the policy installation should succed and the status icon should turn from red circle with a white X to a green circle with a white check
## Distributing and installing the outbound CA and ICA certificates to the client machines pp. 445-453
1. Minor formatting in second paragraph. The name of the VM is SmartConsole; the object name is SmartConsole_VM.
2. The book instructs the user to browse to the Internet. With the notes provided, this should be working perfectly.
## Enabling Identity Awareness and browser-based authentication pp. 455-459
1. Page 459 paragraph directs the user to access to access https://200.100.0.1/connect, but there are two issues
    - this does not work correctly until the policy is pushed
    - there is a certificate error; the previously installed certificates didn't resolve this
## Creating and using access roles pp. 459-460
1. If you have issus loading the AD objects on page 460
    - try closing out of SmartConsole, then try again
    - make sure ADDCDNS is up and responding
## Testing access role-based rules pp. 461-464
1. If your connection from SmartConsole as user `itadmin` can access facebook initially
    - continue testing and come back to re-test `itadmin` on SmartConsole with facebook later. It does work, if not right away
    - Take the time to look carefully how the logs look when Facebook is blocked for itadmin
