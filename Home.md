# Alces Software Knowledgebase

## Hardware
***
### Servers
  * Dell PowerEdge
    * C-series
      * [[C6220|server/dell/cseries/c6220]]
      * [[C4130|server/dell/cseries/c4130]]
      * [[C6320|Dell/C6320-server]]
    * FX-series
      * [[FC430|server/dell/fxseries/fc430]]
    * M-series
    * T-series
      * [[T630|server/dell/tseries/r630]]
    * R-series
      * [[R420|server/dell/rseries/r420]]
      * [[R430|Dell/R430-server]]
      * [[R515|server/dell/rseries/r515]]
      * [[R630|server/dell/rseries/r630]]
      * [[R720|server/dell/rseries/r720]]
    * Integrated Dell Remote Access Controller ([[iDRAC|server/dell/idrac]])
  * HP Proliant
    * Legacy-BIOS configuration:
      * All [[HP Gen9 servers|server/hp/gen9-legacy]] (Haswell/Broadwell CPU)
    * UEFI-BIOS configuration:
      * All [[HP Gen9 servers|server/hp/gen9-uefi]] (Haswell/Broadwell CPU)
    * Legacy-BIOS configuration:
      * SL-series
        * [[apollo2000|server/hp/apollo/2000]]
      * DL-series
        * [[DL360G9|server/hp/proliant/dl360g9]]
        * [[DL380G9|server/hp/proliant/dl380g9]]
      * BL-series
        * [[C7000|server/hp/proliant/c7000]] 10U Blade chassis
        * [[BL460cG8|server/hp/proliant/bl460cg8]]
        * [[BL460cG9|server/hp/proliant/bl460cg9]]
  * SuperMicro
    * SuperServer
      * [[X10DRT-PIBF|server/supermicro/superserver/x10drt-pibf]]

***
### Storage
  * Dell
     * PowerEdge RAID Controller (PERC)
        * PERC [[H200|storage/dell/internal/h200]] and SAS HBAs
        * PERC [[H300|storage/dell/internal/h200]] and H330
        * PERC [[H700|storage/dell/internal/h200]], H730, H800 and H830
     * PowerVault series
        * [[MD3060e|storage/dell/md3060e]] SAS expansion enclosure
        * [[MD34x0|storage/dell/md34x0]] SAS arrays (MD3400, MD3420, MD3460)
        * [[MD3660i|storage/dell/md36x0i]] iSCSI array
     * HP
        * [[MSA2040|storage/hp/msa2040]] SAS array
  * Panasas
  * Seagate/Xyratex

***
### Network switches
  * Ethernet managed switches
    * Dell
      * [[PC2848|network/ethernet1/dell/pc2848]] 1Gb switch
      * [[PC3548|network/ethernet1/dell/pc3548]] 100baseT switch
      * [[PC5548|network/ethernet1/dell/pc5548]] 1Gb switch with 10Gb ports
      * [[PC6248|network/ethernet1/dell/pc6248]] 1Gb switch with module slots
      * [[N1000/N2000 series|network/ethernet1/dell/n2048]] 1Gb Ethernet switches
      * [[N3000/N4000 series|network/ethernet1/dell/n4032]] 10Gb Ethernet switches
      * [[S6000|network/ethernet1/dell/S6000]] 40Gb Ethernet switch
      * [[S6010|network/ethernet1/dell/S6010]] 40Gb Ethernet switch
    * HP
      * [[HP2920|network/ethernet1/hp/2929]] 1Gb 48-port switch
      * [[HP5900|network/ethernet1/hp/5900]] 10Gb 48-port switch with QSFP ports
    * Cisco
  * Infiniband
    * Qlogic/Intel
      * [[(QL)12200 and 12300|network/infinibandQ/intel/12200]]
    * Mellanox
      * [[EDR Switch-IB SB7800|network/infiniband/mellanox/sb7800]]

***
### Power distribution units
  * Dell
    * [[Dell|pdu/dell/dell]]
  * APC
    * [[APC 8653|pdu/apc/AP8653]]
    * [[APC 8886|pdu/apc/AP8886]] 3ph 32A monitored PDU
  * HP
    * [[HP Intelligent PDU|pdu/hp/hpipdu]]



***
### Hardware burn-in testing
  * Basic server burn-in
    * [[Profile:Basic|burnin/server/basic]]
  * Extended server burn-in
    * [[Profile:Extended|burnin/server/extended]]
  * Basic fabric burn-in
    * [[Profile:Fabric-Basic|burnin/fabric/basic]]
  * Extended fabric burn-in
    * [[Profile:Fabric-Extended|burnin-fabric-extended]]

##### Hardware burn-in test documentation
   * [[Memtester|Burn-In-Tests:-Memtester]]
   * [[HPL|Burn-In-Tests:-HPL]]
   * [[IOZone|Burn-In-Tests:-IOZone]]
   * [[IMB|Burn-In-Tests:-IMB]]
   * [[GPUBurn|Burn-In-Tests:-GPUBurn]]

## Software

### User management
   * [[FreeIPA|Software:-FreeIPA]]

### Operating System
   * [[OpenStack ready images|Software:-OpenStackImages]]

### Schedulers
   * [[Slurm|schedulers/slurm]]