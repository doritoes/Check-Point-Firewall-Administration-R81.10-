# Tips
## vCPU and RAM Budgeting
The author has carefully sized the lab's virtual machines for optimal performance. Trying to tweak my setup I ran into issues:
1. Slowness and crashes in part caused by over-allocation of CPU cores and RAM to VMs
2. Type 2 hypervisors have limited support for memory sharing
    - Without memory sharing, it's possible cause paging to disk
    - VirtualBox does support memory balooning, but should not have to for this lab.

vCPU
|  VM                      |  vCPU  |  Count  |  Total  |
|---|---:|---:|---:|
| Router                   |      2 |       1 |       2 |
| Windows VM SmartConsole  |      2 |       1 |       2 |
| Windows ADDCDNS          |      2 |       1 |       2 |
| Windows VMs remaining    |      1 |       3 |       3 |
| Check Point SMS          |      4 |       1 |       4 |
| Check Point Gateways     |      4 |       3 |      12 |
|---|---|---|---|
| Total                    |        |         |      24 |

RAM
|  VM           |  RAM GB  |
|---|---:|
| Router        |        1 |
| SmartConsole  |        4 |
| ADDCDNS       |        2 |
| DMZSRV        |        1 |
| LeftHost      |        1 |
| RightHost     |        1 |
| CPSMS         |        6 |
| CPCM1         |        4 |
| CPCM2         |        4 |
| CPGW          |        4 |
|---|---|
| Total         |       28 |

Disk

|  VM                   |  Disk GB  |  Count  |  Total  |
|---|---:|---:|---:|
| Router                |         8 |       1 |       8 |
| Windows VMs           |        40 |       5 |     200 |
| Check Point SMS       |        60 |       1 |      60 |
| Check Point Gateways  |        60 |       3 |     180 |
|---|---|---|---|
| Total                 |           |         |  448 GB |

Comparing this to my test system, HP Omen laptop:
    - RAM: 32GB
    - Processor: Intel® Core™ i7-12700H Processor
        - Total cores
        - Number of performance-cores: 6
        - Number of efficient-cores: 8
        - Total threads: 20

Leaves 4GB for the host and 4 threads short.

## Windows Sessions Manager Crashing
1. Symptoms: screen goes black, eventually logs out; screen doesn't update until the mouse is clicked
    - If you scripted WINBASE, try re-installing Guest Additions using the Guest Additions CD Image; see pages 107-108
## Usability
1. Increase the default font size in putty
    - launch putty
    - under saved sessions, note Default Settings
    - in the tree, under Window find Apperance, and click it
    - under Font settings, click Change
    - set font (a monospace font like Consolas or Courier New) and font size
    - click ok
    - back in the tree, click on Session
    - click on Default Settings
    - click Save
    - this does not modify previously saved sessions
## Tricks
1. Set the time zone on WINBAS before you clone it
2. Check if the management server is completely up (works for SMS and MDS)
    - `$MDS_FWDIR/scripts/check_cpm_status.sh`
3. You can try using `certutil` to install your custom root certificates (pp. 441-452) using the command line
    - `certutil -enterprise -f -v -AddStore "Root" <Cert File path>`
