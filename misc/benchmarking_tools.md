## Using `iperf` test bandwidth between two end hosts.
Since TCP may automatically handles the congestion and controls the transmission speed which may fail to show the correct bandwidth, we should use UDP to test the throughput. We can use the following command to conduct the tests:
```bash
# server side
$ iperf -u -s

# client side
$ iperf -c 10.0.0.1 -u -b 100m
```
In the client side command, `-b` indicates the estimated bandwidth between the two end hosts.
