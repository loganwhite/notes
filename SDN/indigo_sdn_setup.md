## add flows while system starts up
Note: only local folder can reserve data after reboot.

1. add a shell script in `/local` folder named `add_flows.sh`

2. add the following code:

```bash
echo "wait for open flow daemon to start..."
sleep 1m

dpctl add-flow tcp:127.0.0.1:6633 priority=100,idle_timeout=0,hard_timeout=0,in_port=51,dl_type=0x800,nw_src=10.0.0.1/32,actions=mod_dl_dst:7c:fe:90:91:b7:70,output:8
dpctl add-flow tcp:127.0.0.1:6633 priority=100,idle_timeout=0,hard_timeout=0,in_port=51,dl_type=0x800,nw_src=10.0.0.2/32,actions=mod_dl_dst:7c:fe:90:91:b7:70,output:18
dpctl add-flow tcp:127.0.0.1:6633 priority=100,idle_timeout=0,hard_timeout=0,in_port=51,dl_type=0x800,nw_src=10.0.0.3/32,actions=mod_dl_dst:7c:fe:90:91:b7:70,output:38

dpctl add-flow tcp:127.0.0.1:6633 priority=100,idle_timeout=0,hard_timeout=0,in_port=8,actions=mod_dl_dst:e4:1d:2d:29:f0:40,output:51
dpctl add-flow tcp:127.0.0.1:6633 priority=100,idle_timeout=0,hard_timeout=0,in_port=18,actions=mod_dl_dst:e4:1d:2d:29:f0:40,output:51
dpctl add-flow tcp:127.0.0.1:6633 priority=100,idle_timeout=0,hard_timeout=0,in_port=28,actions=mod_dl_dst:e4:1d:2d:29:f0:40,output:51

dpctl dump-flows tcp:127.0.0.1:6633
```
3. add `indigo-postinit.sh`, and type the followig code:
```bash
#!/bin/sh

source /local/add_flows.sh &
```
Note that absolute path must be used and `&` must be used to put the `add_flows.sh` script run in the background and allow openflow daemon to start.
