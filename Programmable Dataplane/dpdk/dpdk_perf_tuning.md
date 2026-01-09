# Tuning Performance for 100Gbps DPDK Programs
Tuning DPDK programs comes with lots of efforts, and there are pits and falls to be aware of.

## 1. Hardware platform requirements
Recommended hardware platforms includes **PCIe 4.0 X16** 100Gbps NICs and PCIe 4.0 capable CPU and motherboard. It is noted that PCIe 4.0 X16 is a must to attain 148Mpps on both RX and TX due to PCIe TLP overhead (including 16Byte header, 4Byte LCRC, 4Byte Seq/Frameing), DLLP overhead (5-10% tput overhead), Root Complex overhead (15%-20%), and padding overhead. Hence, on my platform, the maximum speed achieved using `testpmd` is 136.2Mpps on both side.

## 2. Hardware parameters
Hardware parameters should be configured before conducting software tests.
2.1. `DDIO` should be enabled in BIOS. This is the most important option to send packets direct to the CPU cache.
2.2. Enable Relaxed Ordering (some called no snoopy) in PCIE settings in BIOS. This allows the PCIe device will not be aware or the order of PCIe transaction packet orders. This option is the base for enabling relaxed ordering in software settings.
## 3. Software platform level parameters
Software platform level parameters includes OS parameters and NIC settings.
OS parameters:
1. **Hugepage setup.**
   2M hugepage size should be enough. I have encoutered issues that TREX traffic generator response with no enought hugepages when setting 1G hugepage size even I have used 100GB physical memory for huge page, but the 2M huge page works fine. I don't see obvious performance differences between these two kinds. The commands are listed as follows.
   ```bash
   sudo rm -rf /dev/hugepages/*
   sysctl vm.nr_hugepages=20480
   ```
2. **System parameters.**
   Sometimes, OS task scheduler may impact the DPDK slave core processing packets. Hence, we can isolate DPDK used slave cores from the OS's regime.
   in `/etc/default/grub`, change `GRUB_CMDLINE_LINUX_DEFAULT` to `GRUB_CMDLINE_LINUX_DEFAULT="quiet splash isolcpus=managed_irq,domain,1-23 nohz_full=1-23 rcu_nocbs=1-23 rcu_nocb_poll intel_idle.max_cstate=0 processor.max_cstate=0 intel_pstate=disable audit=0 nosoftlockup vt.handoff=1"`. After that the following commands are required.
   ```bash
   sudo update-grub
   sudo reboot
   ```
3. **NIC configuration.**
   We are using the Mellanox ConnectX-5 NIC MLX516A CCAT dual-port 100 Gbps NIC. It has multiple tuning parameters.
   3.1. Installing the OFED driver (now becomes the nvidia doca drivers) and firmware.
   3.2. Disabling flow control (pause frame) and N-Tuple Filters (aka. Receive Flow Steering) using `ethtool`
   ```bash
   ethtool -K ens3f1np1 ntuple off
   ethtool -A ens3f1np1 rx off tx off
   ```
   3.3. Setup PCIe header size. On some machines, depends on the machine integrator, it is beneficial to set the PCI max read request parameter to 1K.
   ```bash
   # To query the read request size use:
   setpci -s <NIC PCI address> 68.w
   # If the output is different than 3XXX, set it by:
   setpci -s <NIC PCI address> 68.w=3XXX
   ```
   3.4. Setup CQE zipping. `mlxconfig -d <mst device> s CQE_COMPRESSION=1`. MST device can be obtained by `mst status`. If mst device is not started, run it with `mst start`.
   3.5. Setup Relaxed Ordering for the NIC.
   ```bash
   mlxconfig -d 4b:00.1 set PCI_WR_ORDERING=1
   ```
4. **DPDK program parameter.** Using `testpmd` as an example. The following command parameters are used
   ```bash
   ./testpmd -l 0,2,4,6,8,10,12,14,16,18,20,22,24,26,28,30 -n 4 -w 4b:00.1,rxq_cqe_comp_en=1,mprq_en=1,rxqs_min_mprq=1,mprq_max_memcpy_len=128,txq_inline_max=64,txq_inline_mpw=64,mprq_log_stride_num=8 --socket-mem=4096,0  -- --rxq=12 --txq=12 --nb-cores=12 --rxd=1024 --txd=1024 --mbcache=512 --burst=64 --rss-ip --forward-mode=io --stats-period=1 --interactive --no-flush-rx --total-num-mbufs=5242808
   ```
`rxq_cqe_comp_en=1`: enables CQE compression.

`mprq_en=1`: enables Multi-Packet Receive Queue.

`rxqs_min_mprq=1`: defines the minimum number of Multi-Packet Receive Queue.

`mprq_max_memcpy_len=128`: copy bulk memory of MPRQ from NIC to host memory mbuf, and when the value is greater than 128 (not including), the copy is not enabled.

`txq_inline_max=64`: inline 64 Byte packet value to a WQE (send control and data together)

`txq_inline_mpw=64`: for Enhanced Multi-Packet Write (eMPW), allows multiple packets to be sent with one PCIe transaction.

`--rxq=12 --txq=12 --nb-cores=12 --rxd=1024 --txd=1024 --mbcache=512 --burst=64` parameters defines the number of rx and tx queues used and the # of cores used. `--rxd=1024 --txd=1024` defines the number of hardware descriptors in each rx ring and tx ring. If you encounter packet loss, this value should be increased. `--mbcache=512` sets the mbuf cache size for each core.

To prevent crashes or packet loss, your Total Mempool Size must be significantly larger than the sum of these parameters.

The Safety Formula:

Total Mbufs>(RXD+TXD)×Queues+(mbcache×Lcores)+Burst_Overhead

The Performance Trade-off:

RXD/TXD vs. Mbcache:

Larger RXD/TXD means Better ability to absorb traffic bursts (micro-bursts), but places higher pressure on memory capacity.

Larger Mbcache means Faster allocation/freeing of memory (hits L1/L2 cache, no locks), leading to higher throughput.

However, you cannot make mbcache arbitrarily large. DPDK typically requires that the cache size is not greater than roughly  1/1.5 of the total pool size per pool.

Mbcache vs. Burst Size:

If your application processes packets in bursts of 32, a cache of 512 is excellent (512/32=16 rounds).

If mbcache is too small (e.g., smaller than the burst size), the CPU constantly has to go to the Global Mempool (hitting locks), which kills performance.

If you see imiss (Hardware drops): Your CPU might be too slow to empty the RX ring, OR your memory pool is too small (Starvation).

If CPU usage is low but throughput is capped: Check if mbcache is too small, causing lock contention on the memory pool.



   
