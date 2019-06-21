## How to run T-rex
1. open a terminal to veda-umh  
   `cd /opt/trex/(version)`  
   `sudo ./t-rex-64 -i -c 4 --mbuf-factor 0.2`

2. Another console to veda-umh  
   `cd /opt/trex/v2.49/`  
   `sudo ./trex-console`  
  
   Then type  
   `tui # Enter the interactive command line user interface`  
   `start -f stl/bench.py -m 100% --port 0 -t size=64,vm=var2 -d 100`
