1. Check the list of supported NICs at http://core.dpdk.org/supported/

2. Verify by comparing the device ID to the supported list. First use the following Linux command:

```lspci –nn | grep eth0 ```

The result shows a PCI ID for the called eth interface :

```04:00.0 Ethernet controller [0200]: Intel Corporation I350 Gigabit Network Connection [8086:xxxx]```

Then take the number ```xxxx``` in the output to check against the current version of ```rte_pci_dev.h``` in the SDK to see if your NIC is supported.
