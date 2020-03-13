## add flows while system starts up
Note: only local folder can reserve data after reboot.
Reference: [indigo manual](https://floodlight.atlassian.net/wiki/spaces/Indigo/pages/2392083/Indigo+Initialization+and+Configuration) describes the system initialization process

1. add a shell script in `/local` folder named `add_flows.sh`

2. add the following code:

```bash
#!/bin/sh


echo "wait for open flow daemon to start..."
sleep 1m

dpctl add-flow tcp:127.0.0.1:6633 priority=100,idle_timeout=0,hard_timeout=0,in_port=51,dl_type=0x800,nw_src=10.0.0.1/32,actions=mod_dl_dst:00:00:00:00:00:00,output:8
dpctl add-flow tcp:127.0.0.1:6633 priority=100,idle_timeout=0,hard_timeout=0,in_port=51,dl_type=0x800,nw_src=10.0.0.2/32,actions=mod_dl_dst:00:00:00:00:00:00,output:18
dpctl add-flow tcp:127.0.0.1:6633 priority=100,idle_timeout=0,hard_timeout=0,in_port=51,dl_type=0x800,nw_src=10.0.0.3/32,actions=mod_dl_dst:00:00:00:00:00:00,output:38

dpctl add-flow tcp:127.0.0.1:6633 priority=100,idle_timeout=0,hard_timeout=0,in_port=8,actions=mod_dl_dst:00:00:00:00:00:00,output:51
dpctl add-flow tcp:127.0.0.1:6633 priority=100,idle_timeout=0,hard_timeout=0,in_port=18,actions=mod_dl_dst:00:00:00:00:00:00,output:51
dpctl add-flow tcp:127.0.0.1:6633 priority=100,idle_timeout=0,hard_timeout=0,in_port=28,actions=mod_dl_dst:00:00:00:00:00:00,output:51

dpctl dump-flows tcp:127.0.0.1:6633
```
3. add `indigo-postinit.sh`, and type the followig code:
```bash
#!/bin/sh

source /local/add_flows.sh &
```
Note that absolute path must be used and `&` must be used to put the `add_flows.sh` script run in the background and allow openflow daemon to start.

## limit forwarding ports speed
using the `port` command, as shown in [Indigo Manual](https://floodlight.atlassian.net/wiki/spaces/Indigo/pages/2392084/Indigo+CLI+Reference)
```bash
cmd port set 8,18,38 speed=100
```
**After limiting the speed using the command above, we have to manually enable the set ports**
```bash
cmd port set 8,18,38 enable=yes
cmd port set 8,18,38 autoneg=yes
```

**NOTE:** that if using two indigo switches connecting together, one have to be **not enable*** `autoneg`, otherwise, the speed limit will not be enabled.

**NOTE:** if you do not limit port speed. Left everything as default and don't have to set something specifically.

Keyword `cmd` should be used here to invoke `port` command. The unit of speed is Mb.
