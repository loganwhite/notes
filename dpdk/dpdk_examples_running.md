## L3FWD running on mellanox nic equipped devices
```bash
sudo build/l3fwd -l 1,3,5,7,9 -n 4 -w af:00.0,mprq_en=1 -- \
      -p 0x01 \
      -P \
      --config="(0,0,1),(0,1,3),(0,2,5),(0,3,7),(0,4,9)" \
      --eth-dest=0,00:52:11:22:33:10
```

The –config option enables one queue on each port and maps each (port,queue) pair to a specific core. The following table shows the mapping in this example:

Port	Queue	lcore	Description
```
0	0	1	Map queue 0 from port 0 to lcore 1.
1	0	2	Map queue 0 from port 1 to lcore 2.
```
```bash
EAL: Detected 48 lcore(s)
EAL: Detected 2 NUMA nodes
EAL: Multi-process socket /var/run/dpdk/rte/mp_socket
EAL: Probing VFIO support...
EAL: PCI device 0000:af:00.0 on NUMA socket 1
EAL:   probe driver: 15b3:1019 net_mlx5
LPM or EM none selected, default LPM on
Initializing port 0 ... Creating queues: nb_rxq=5 nb_txq=5...  Address:50:6B:4B:D3:C1:48, Destination:00:52:11:22:33:10, Allocated mbuf pool on socket 1
LPM: Adding route 0x01010100 / 24 (0)
LPM: Adding route IPV6 / 48 (0)
txq=1,0,1 txq=3,1,1 txq=5,2,1 txq=7,3,1 txq=9,4,1

Initializing rx queues on lcore 1 ... rxq=0,0,1
Initializing rx queues on lcore 3 ... rxq=0,1,1
Initializing rx queues on lcore 5 ... rxq=0,2,1
Initializing rx queues on lcore 7 ... rxq=0,3,1
Initializing rx queues on lcore 9 ... rxq=0,4,1


Checking link statusdone
Port0 Link Up. Speed 100000 Mbps -full-duplex
L3FWD: entering main loop on lcore 3
L3FWD:  -- lcoreid=3 portid=0 rxqueueid=1
L3FWD: entering main loop on lcore 5
L3FWD:  -- lcoreid=5 portid=0 rxqueueid=2
L3FWD: entering main loop on lcore 1
L3FWD:  -- lcoreid=1 portid=0 rxqueueid=0
L3FWD: entering main loop on lcore 7
L3FWD:  -- lcoreid=7 portid=0 rxqueueid=3
L3FWD: entering main loop on lcore 9
L3FWD:  -- lcoreid=9 portid=0 rxqueueid=4
...
```
