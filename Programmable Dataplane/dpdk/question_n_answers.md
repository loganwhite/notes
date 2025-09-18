## `rte_pktmbuf_pool_create` return NULL

We can check the error code to see peak what is the reason for that. Sometimes, if the error code is 22, which is `EINVAL`, this is because of the cache size provided is too large for the per-core cache pool size on this machine. To manually increase this value, we have to change the DPDK distribution setting in the `.config` file in the installation folder (*e.g.*, `x86_64-native-linuxapp-clang`). Change `CONFIG_RTE_MEMPOOL_CACHE_MAX_SIZE` to the number you wish to.


## v16.07 or v17.11 to v18.11

The following modification can be conducted
```c
static const struct rte_eth_conf port_conf = {
        .rxmode = {
                 .mq_mode        = ETH_MQ_RX_RSS,
                 .max_rx_pkt_len = ETHER_MAX_LEN,
-                 .split_hdr_size = 0,
-                 .header_split   = 0,                    /* header split disabled */
-                 .hw_ip_checksum = 1,                    /* IP checksum offload enabled */
-                 .hw_vlan_filter = 0,                    /* VLAN filtering disabled */
-                 .jumbo_frame    = 0,                    /* jumbo frame support disabled */
-                 .hw_strip_crc   = 1,                    /* CRC stripped by hardware */
+                 .offloads       = DEV_RX_OFFLOAD_CHECKSUM,
         },
         .rx_adv_conf = {
                 .rss_conf = {
                         .rss_key = rss_symmetric_key,
                         .rss_hf  = ETH_RSS_IP | ETH_RSS_UDP | ETH_RSS_TCP,
                 },
         },
         .txmode = {
                 .mq_mode = ETH_MQ_TX_NONE,
                 .offloads = (DEV_TX_OFFLOAD_IPV4_CKSUM |
                              DEV_TX_OFFLOAD_UDP_CKSUM  |
                              DEV_TX_OFFLOAD_TCP_CKSUM)
         },
 };
 
-         total_ports = rte_eth_dev_count();
+         total_ports = rte_eth_dev_count_avail();
 
```

## How to compile DPDK that generates debug symbols
add the following line before compiling DPDK
```bash
export EXTRA_CFLAGS='-O0 -g'
```

## Get Segmentfault (Core dump) after allocating memory with `rte_malloc`
This is really a stupid issue I faced more than once. The first time it the becomes OK but I didn't know why. The second time, I figured out that it was because i forget to add the `rte_malloc.h` header file in the source code.


## `RING: Cannot reserve memory` and `HASH: memory allocation failed` when creating Hash Table on different CPU sockets
The most possible reason for show this may be the name of the hash tables are duplicate, thus resulting in memory region confilict.
**Use different Hash names for them**.


## How to calculate CPU utilization and queue utilization in DPDK?
As DPDK using the poll model, and the CPU keeps polling the NIC to get new packets. This results in the CPU utilization is always 100% at the operating system level. In fact, the CPU utilization should be calculated as # of CPU cycles that receives packets over # of packets of total packets. The following code snippet can be used for getting the CPU utilization (not yet validated; code from RSS++).
```c++
...
Vector<float> l(num_max_cpus(), 0);
unsigned long long totalUser, totalUserLow, totalSys, totalIdle, totalIoWait, totalIrq, totalSoftIrq;
int cpuId;
FILE* file = fopen("/proc/stat", "r");
char buffer[1024];
char *res = fgets(buffer, 1024, file);
assert(res);
while (fscanf(file, "cpu%d %llu %llu %llu %llu %llu %llu %llu", &cpuId, &totalUser, &totalUserLow, &totalSys, &totalIdle, &totalIoWait, &totalIrq, &totalSoftIrq) > 0) {
if (cpuId < l.size()) {
        unsigned long long newTotal = totalUser + totalUserLow + totalSys + totalIrq + totalSoftIrq;
        unsigned long long tdiff =  (newTotal - _cpustats[cpuId].lastTotal);
        unsigned long long idiff =  (totalIdle - _cpustats[cpuId].lastIdle);
        if (tdiff + idiff > 0)
            l[cpuId] =  (float)tdiff / (float)(tdiff + idiff);
        _cpustats[cpuId].lastTotal = newTotal;
        _cpustats[cpuId].lastIdle = totalIdle;
        //click_chatter("C %d total %d %d %d",cpuId,newTotal,tdiff, idiff);
        res = fgets(buffer, 1024, file);
    }
}
fclose(file);
...
```
Also the queue utilization can be retrieved by calculating the # of descriptors has recevied on the queue over # of total descriptors on the queue (not validated, code from RSS++).
```c++
...
DPDKDevice* fd = (DPDKDevice*)((BalanceMethodDevice*)_method)->_fd;
int port_id = fd->port_id;
float rxdesc = fd->get_nb_rxdesc();
for (int u = 0; u < _used_cpus.size(); u++) {
    int i = _used_cpus[u].id;
    int v = rte_eth_rx_queue_count(port_id, i);
    float l = (float)v / rxdesc;
    load.push_back(std::pair<int,float>{i,l});
    totload += l;
}
...
```

