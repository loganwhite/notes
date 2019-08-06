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

## dest MAC address is invalid error
Restart the trex server


## Port 0 : *** port is already owned by another session of 'root'
Do not use sudo to run the trex server when you are already a root user.


## When writing TRex python scripts, add the TRex libraries to python env.

```bash
export PYTHONPATH="${PYTHONPATH}:/opt/trex/v2.59/automation/trex_control_plane/interactive"
```
