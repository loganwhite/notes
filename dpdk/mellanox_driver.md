When installing the Mellanox OFED driver. we usually use the ```./mlnxofedinstall --upstream-libs --dpdk``` commands. Parameters ```--upstream-libs --dpdk``` shows that user space libraries that are needed for Mellanox PMDs will be installed.


Sometimes, when installing the Mellanox OFED driver, Firmware installation might be failed. In this case, we can try to use the Mellanox Firmware Tool (MFT) to install the Firmware required.

## Getting PSID from MFT:
1. Enter: `mst start`
2. Get the Mellanox `mst device` name using the command `mst status`. 
The device name will be of the form: `dev/mst/mt<dev_id>_pci{_cr0|conf0}` 	 
3. Get the PSID (firmware identification) using the command: `flint -d /dev/mst/mt4099_pci_cr0 query`
```
Image type:            FS2
FW Version:            2.42.5000
FW Release Date:       5.9.2017
Product Version:       02.42.50.00
Rom Info:              type=PXE version=3.4.752
Device ID:             4103
Description:           Node             Port1            Port2            Sys image
GUIDs:                 ffffffffffffffff ffffffffffffffff ffffffffffffffff ffffffffffffffff
MACs:                                       7cfe9091b930     7cfe9091b931
VSD:
PSID:                  MT_1480111023
```

