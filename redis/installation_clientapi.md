# Redis deployment and client api for C

## Redis server build and installation
1. Download the redis in-memory db from https://www.redis.io
2. Uncompress the `*.tar.gz` file and build redis (build-essential package needed on ubuntu) 
  1. Compile redis with `make`. 
3. Run `src/redis-server` to start the server 
4. Run `src/redis-cli` to start the build-in command line client 
## Redis Client build, installation, and usage.
1. Clone hiredis client API library from github:https://github.com/redis/hiredis 
2. Make and install: `make && sudo make install`. 
3. `ldconfig -p | grep hiredis` to check out if the library is installed successfully. 
If prints nothing, add `“include /usr/local/lib”` to file `“/etc/ld.so.conf”` and validate the configuration with `“sudo ldconfig”`. 
4. To use hiredis in our own C program, add `“#include <hiredis/hiredis.h>”`. 
5. Instruction follow https://github.com/redis/hiredis 
6. Compile with the command `gcc sourcefile –lhiredis –o outputfile`. 
7. Redis-server disable protected mode: Enter `CONFIG SET protected-mode no` in the redis command line. 
8. host traffic control: `tc qdisc add dev eth0 root netem delay 10ms`
