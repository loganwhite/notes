## `rte_pktmbuf_pool_create` return NULL

We can check the error code to see peak what is the reason for that. Sometimes, if the error code is 22, which is `EINVAL`, this is because of the cache size provided is too large for the per-core cache pool size on this machine. To manually increase this value, we have to change the DPDK distribution setting in the `.config` file in the installation folder (*e.g.*, `x86_64-native-linuxapp-clang`). Change `CONFIG_RTE_MEMPOOL_CACHE_MAX_SIZE` to the number you wish to.
