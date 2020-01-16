## Using L3 protocol like IP should specify the protocol type in layer 2

```bash
dpctl add-flow tcp:127.0.0.1:6633 priority=100,idle_timeout=0,hard_timeout=0,in_port=51,dl_type=0x800,nw_src=10.0.0.1/32,actions=mod_dl_dst:7c:fe:90:91:b7:70,output:8
```
Here, `dl_type=0x800` is needed.

## How to set up a flow never expires.
`idle_timeout=0,hard_timeout=0`
