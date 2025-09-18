# Sending Packets between two Physical Host via the DPDK Binded Nics

## Using BESS as a traffic generator and dpdk-pktgen as a receiver monitor.
1. Connect the DPDK compatible nics with a wire directly.
2. Bind the two nics with DPDK compatible drivers with the tools provided by DPDK located in ```$DPDK_SDK/usertools/dpdk-devbind.py```
3. Install BESS on the sender machine and install dpdk-pktgen on the receiver machine.
4. On the BESS machine:
  ..* Start the ```bessd``` daemon.
  ..* Start the ```bessctl``` commandline.
  ..* Write the BESS configuration code as follows:
      ```
      phy_port = PMDPort(port_id=0,num_inc_q=1, num_out_q=1)
      output = QueueOut(port=phy_port, qid=0) #This module will write to myport on queue

      Source() -> Measure() -> output
      ```
  ..* Run the BESS configuration script with ```run script_path/script_name.bess``` in ```bessctl``` commandline.
  ..* ```run monitor pipeline``` to monitor the running the script.
5. On the dpdk-pktgen machine:
  ..* run ```./app/x86_64-native-linuxapp-gcc/pktgen -l 0-4 -n 3 -- -P -m "[1:3].0"``` (in this example, we only run dpdk-pktgen on the first dpdk port)
  ..* enter ```start all ``` in the dpdk-pktgen commandline.

Now you can monitor the pkts are sending in the BESS machine and pkts are receiving in the dpdk-pktgen machine.

## Using BESS and Trex send from one port and rececive from another.
1. most of the configurations are the same as previous section except the follows:
2. Compile and run Trex:
  1. open one terminal to the trex machine  
   `cd /opt/trex/script`  
   `sudo ./t-rex-64 -i -c 4 --mbuf-factor 0.2`

  2. Another console to the trex machine  
     `cd /opt/trex/script/`  
     `sudo ./trex-console`  

     Then type  
     `tui`  
     `start -f stl/bench.py -m 100% --port 0 -t size=64,vm=var2 -d 10`
3. Change the BESS configuration script as follows:
    ```
    phy_port1 = PMDPort(port_id=0,num_inc_q=1, num_out_q=1)
    phy_port2 = PMDPort(port_id=1,num_inc_q=1, num_out_q=1)

    input1 = QueueInc(port=phy_port1, qid=0)

    output2 = QueueOut(port=phy_port2, qid=0) #This module will write to myport on queue

    input1 -> Measure() -> output2
    ```
