# Dell Power Edge C6220

## Profile 'Cluster Slave'

### BIOS
1. Power on and press F2 to enter BIOS, note the firmware revisions during boot
2. Enter first pass BIOS and configure the following:
```
Exit -> Load Optimal Defaults -> “yes”
```
```
Advanced -> Power Management -> “Maximum Performance”
```
```
Advanced -> CPU Configuration -> C­States -> “Disabled”
```
```
Advanced -> CPU Configuration -> Hyperthreading/SMT -> “off”
```
```
Advanced -> CPU Configuration -> Turbo Mode -> “On”
```
```
Boot -> Quiet Boot -> “Disabled”
```
```
Server -> Set BMC LAN Configuration -> BMC LAN Port Configuration “Dedicated-NIC”
```
```
Server -> Restore on AC Power Loss -> “Always Off”
```
```
Exit -> Save Settings and Exit
```
3. Note configuration details on asset record sheet

### BMC

1. Configure BMC in BIOS as per BMC options in the bios configuration
2. Configure user/password network information via ipmitool using the following parameters
  - LAN_CHANNEL=1
  - ADMIN_USER_ID=2
3. NB password for user must be at least 8 characters, contain capital letters and numbers