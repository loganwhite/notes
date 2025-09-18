## What is the max size of cache size in `rte_mempool_create`?

When using `rte_mempool_create` to create the memory pool. Cache size can be assigned to inprove the performance. the maximum value of this cache size is defined in `CONFIG_RTE_MEMPOOL_CACHE_MAX_SIZE` in the dpdk configuration file (`.config`) under the DPDK installation directory.
