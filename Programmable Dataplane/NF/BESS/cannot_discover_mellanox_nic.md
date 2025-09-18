Sometimes, BESS just cannot recognize the Mellonox driver. The creator the BESS has posted a solution in the issue https://github.com/NetSys/bess/issues/863 as follows:
change the code in ```core/drivers/pmd.cc```
```
ret.rx_adv_conf.rss_conf = {                                        
    .rss_key = nullptr,                                             
-     .rss_key_len = 40,                                              
+     .rss_key_len = 0,                                              
    /* TODO: query rte_eth_dev_info_get() to set this*/             
    .rss_hf = ETH_RSS_IP | ETH_RSS_UDP | ETH_RSS_TCP | ETH_RSS_SCTP,
```
