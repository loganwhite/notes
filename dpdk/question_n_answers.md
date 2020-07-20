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
