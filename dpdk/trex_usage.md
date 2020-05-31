## A valid TRex configuration file
1. Open file /etc/trex-cfg.yaml
2. Paste the following code snippet
```yaml
- port_limit      : 2
  version         : 2
#List of interfaces. Change to suit your setup. Use ./dpdk_setup_ports.py -s to see available options
  interfaces    : ["04:00.0","dummy"]
  port_info       :  # Port IPs. Change to suit your needs. In case of loopback, you can leave as is.
        - dest_mac: 7c:fe:90:91:b9:10
          src_mac:  7C:FE:90:91:B9:30
  platform:
          master_thread_id: 0
          latency_thread_id: 2
          dual_if:
          - socket: 1
            threads: [1,3,5,7,9,11,13,15,17,19,21,23]
```

## How to run T-rex
1. open a terminal to veda-umh  
```bash
    cd /opt/trex/(version)  
    sudo ./t-rex-64 -i -c 4 --mbuf-factor 0.2
```

2. Another console to veda-umh  
```bash
  cd /opt/trex/v2.49/  
  sudo ./trex-console
```
  
   Then type  
   `tui # Enter the interactive command line user interface`  
   `start -f stl/bench.py -m 100% --port 0 -t size=64,vm=var2 -d 100`

## dest MAC address is invalid error
Restart the trex server


## Port 0 : *** port is already owned by another session of 'root'
Do not use sudo to run the trex server when you are already a root user.


## When writing TRex python scripts, add the TRex libraries to python env.

```bash
export PYTHONPATH="${PYTHONPATH}:/opt/trex/v2.59/automation/trex_control_plane/interactive"
```
## TRex Python script send packets
```python3
key_tuples = [('39.147.185.20','154.98.255.180',6,50377,80),('252.154.243.191','226.241.38.130',6,22516,45885),('34.174.12.235','221.78.250.221',6,4378,80),('249.206.9.207','220.122.203.10',6,1627,80),('224.1.80.204','55.164.223.226',6,49668,10707),('39.221.184.120','220.84.121.133',6,53069,4662)]


# get TRex APIs
from trex_stl_lib.api import *
import threading
import time


def print_stat(c):
    f = open('caida_fluctuation.txt','w')
    times = 600
    while times > 0:
        dut_perf = c.get_stats()[0]['rx_pps']
        send_tput = c.get_stats()[0]['tx_pps']
        print((send_tput, dut_perf))
        f.write("(%s,%s)\n"%(send_tput, dut_perf))
        times -= 1

        time.sleep(1)
    # end while
    f.close()
    exit()

# endof print_stat

c = STLClient(server = '127.0.0.1')
c.connect()
c.reset(ports = [0, 1])

try:
    # imix_table = [ {'size': 64,   'pps': 142000000},
    #                # {'size': 590,  'pps': 20000000},
    #                # {'size': 1514, 'pps': 100000} 
    #                ]

    # create a base pkt
    # base_pkt = Ether()/IP(src="16.0.0.1",dst="48.0.0.9")/UDP(dport=12,sport=1025)
    packets = []
    for item in key_tuples[:100]:
        src_ip = item[0]
        dst_ip = item[1]
        proto = item[2]
        sport = item[3]
        dport = item[4]
        base_pkt = None
        if proto == 6:
            base_pkt = Ether()/IP(src=src_ip,dst=dst_ip)/TCP(dport=dport,sport=sport)
        elif proto == 17:
            base_pkt = Ether()/IP(src=src_ip,dst=dst_ip)/UDP(dport=dport,sport=sport)
        packets.append(base_pkt)

    # stream list
    streams = []

    # # iterate over the IMIX table entries
    # for entry in imix_table:
        # create some padding data to complement the size
    for pkt in packets:
        if not pkt:
            continue
        pad = (64 - len(pkt)) * 'x'

            # create the stream and append it
        streams.append(STLStream(packet = STLPktBuilder(pkt/pad),
                                     mode = STLTXCont(percentage =1)))

    

    # add the streams
    c.add_streams(streams, ports = 0)

    t = threading.Thread(target=print_stat, args=(c,))
    t.start()


    # start traffic with limit of 3 seconds (otherwise it will continue forever)
    # c.start(ports = 0, duration = 10)
    c.start(ports = 0)

    # hold until traffic ends
    c.wait_on_traffic()


except STLError as e:
    print(e)

finally:

    c.disconnect()

```
