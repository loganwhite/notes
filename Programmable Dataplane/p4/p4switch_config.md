# Installing the open-p4studio suit

It should be simply to install open-p4studio but it does not, esp in a network restricted area. The following guide my helpful.

## Setup the network
Some region may block traffic from outside the zone. Hence, a proxy is recommend to deploy on the switch host.
Here i use https://github.com/nelvko/clash-for-linux-install. Please follow the instruction of this repo and install it on the switch host.

## Dry run the installation script and locate the errors.
When running the installation script, multiple errors may occur, for example, libappindicator3-1_0.4.92-7_amd64.deb, the given link is no longer available, and we have to find alternatives.

I changed the p4studio/dependencies/p4i.dependencies.yaml file directly to avoid changing the building and installing mechanisms. The replaced debs can be found in:
http://archive.debian.org/debian/pool/main/liba/libayatana-indicator/
http://archive.debian.org/debian/pool/main/liba/libayatana-appindicator/

After changing the url for missing dependencies, you may find that these links forbids you from downloading the files. The reason is the files are downloaded with Python `urllib` which does not setup *User Agent* by default. Hence we can change the `p4studio/utils/download.py` file as follows:
```python

```

Then, the installation should be smooth. But sometimes, python packages may fail due to timeouts. In such a case, you can manually install the missing packages and it will solve the problem.

## Interruption of the SSH connection.
My installation encountered lost of connection when building `grpc`, the problem may due to memory running out. Hence, **IT IS RECOMMENDED TO RUN THE INSTALLATION SCRIPT IN A SCREEN SESSION**.  
