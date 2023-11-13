# Writing XDP programs comes with 5 steps.
## 0. Prerequisition
Install the necessary compilation tools
```bash
sudo apt install clang
sudo apt install gcc-multilib # in case there's missing header errors.
```
## 1. Write the program.
Create a new `.c` file and add the following code to the file.
```c
#include <linux/bpf.c>
int main() {
  // simply send back the packet
  return XDP_TX;
}
```

## 2. Compile the `.c`file
```bash
clang -target bpf -O2 -c fwd.c # assume the name of the file is fwd.c
```
After the command, you get an object file named `fwd.o`

## 3. Load the BPF program to a link
```bash
ip link set dev ens1np0 xdp obj fwd.o sec .text
```
The command loads the `fwd.o` file to the ens1np0 interface, and specifies the destination to the .text segment in the program memory layout.
**It is noted that there might be cases when error `RTNETLINK error on loading XDP program to ConnectX-5` occurs**. This is because the default MTU of the mellanox nic is too big to be supported by XDP (eg, dmesg shows `MTU(9238) > 3498 is not allowed while XDP enabled`), and we can simply change the MTU using the following command.
```bash
ifconfig ens1np0 mtu 3498
```
Now the BPF program should just work.

## 4. Unload the BPF program
use the following command
```bash
ip link set dev ens1np0 xdp off
```

## Performance evaluation
We can use trex to evaluate the performance of the XDP program. On another server, run the trex, and start sends packets. We can obtain the results from the `tui`.
**It is noted that the `rx` of trex may remain 0**, this can be solved by checking the destination mac address of the packets sent by trex, and the DUT simply ignores packets that does not match its mac address. 
