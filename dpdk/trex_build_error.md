# Error: could not find library 'mlx4'
Go to the https://doc.dpdk.org/guides/nics/mlx4.html page and follow the instructio of Installing Mellanox OFED.
Download the driver and run:
```
$ ./mlnxofedinstall --dpdk --upstream-libs
# modprobe -a ib_uverbs mlx4_en mlx4_core mlx4_ib
```
