# Chapter 13

## Logging into a single security domain pp. 510-524
1. The chapter title appears to be a typo for "Logging in a single security domain"
## Initial configuration pp. 525-528
1. Page 526 step 4 directs the user to open SmartEvent Settings & Policy under External Apps; this app is not present until SmartConsole is closed and opened again
## Initial configuration pp. 528-533
1. Page 529 in last paragraph states there will be two timelines, as seen in Figure 13.20 number 7. This did not appear in testing, so was unable to test.
## Events p. 533
1. The book doesn't intend for you to actually install nmap, but it's worth practicing with it
    - Download from https://nmap.org/download
    - Install on SmartConsole
    - Run `Nmap - Zenmap GUI` (this is the intuitive GUI to nmap)
    - Enter target: `addcdns.mycp.lab` or `10.20.20.10`
    - Profile: Quick Scan
    - Click Scan
    - Wait for the scan to complete
    - Profile: Intense Scan
    - Wait for the scan to complete
