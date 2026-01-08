# Tuning Performance for 100Gbps DPDK Programs
Tuning DPDK programs comes with lots of efforts, and there are pits and falls to be aware of.

## 1. Hardware platform requirements
Recommended hardware platforms includes **PCIe 4.0 X16** 100Gbps NICs and PCIe 4.0 capable CPU and motherboard. It is noted that PCIe 4.0 X16 is a must to attain 148Mpps on both RX and TX due to PCIe TLP overhead (including 16Byte header, 4Byte LCRC, 4Byte Seq/Frameing), DLLP overhead (5-10% tput overhead), Root Complex overhead (15%-20%), and padding overhead. Hence, on my platform, the maximum speed achieved using `testpmd` is 136.2Mpps on both side.

## 2. Hardware parameters
Hardware parameters should be configured before conducting software tests.
1. `DDIO` should be enabled in BIOS. This is the most important option to send packets direct to the CPU cache.
2. Enable Relaxed Ordering (some called no snoopy) in PCIE settings in BIOS. This allows the PCIe device will not be aware or the order of PCIe transaction packet orders. This option is the base for enabling relaxed ordering in software settings.
## 3. Software platform level parameters
Software platform level parameters includes OS parameters and NIC settings.
OS parameters:
1. Hugepage setup.
   2M hugepage size should be enough. I have encoutered issues that TREX traffic generator response with no enought hugepages when setting 1G hugepage size even I have used 100GB physical memory for huge page, but the 2M huge page works fine. I don't see obvious performance differences between these two kinds. The commands are listed as follows.
   ```bash
   sudo rm -rf /dev/hugepages/*
   sysctl vm.nr_hugepages=20480
   ```
2. System parameters.
   Sometimes, OS task scheduler may impact the DPDK slave core processing packets. Hence, we can isolate DPDK used slave cores from the OS's regime.
   in `/etc/default/grub`, change `GRUB_CMDLINE_LINUX_DEFAULT` to `GRUB_CMDLINE_LINUX_DEFAULT="quiet splash isolcpus=managed_irq,domain,1-23 nohz_full=1-23 rcu_nocbs=1-23 rcu_nocb_poll intel_idle.max_cstate=0 processor.max_cstate=0 intel_pstate=disable audit=0 nosoftlockup vt.handoff=1"`. After that the following commands are required.
   ```bash
   sudo update-grub
   sudo reboot
   ```
3. NIC configuration.
