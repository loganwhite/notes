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
import shutil
import tempfile
from urllib import request

from utils.exceptions import ApplicationException
from utils.terminal import compact_log


def download(url: str) -> str:
    file_name = tempfile.mkdtemp() + '/' + url.split("/")[-1]
    try:
        compact_log().log("downloading: {}".format(url))
        req = request.Request(url, headers={"User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/18.6 Safari/605.1.15"})
        with request.urlopen(req) as response, open(file_name, 'wb') as out_file:
            shutil.copyfileobj(response, out_file)
    except Exception as e:
        compact_log().log(str(e))
        raise ApplicationException from e
    return file_name
```

Then, the installation should be smooth. But sometimes, python packages may fail due to timeouts. In such a case, you can manually install the missing packages and it will solve the problem.

## Interruption of the SSH connection.
My installation encountered lost of connection when building `grpc`, the problem may due to memory running out. Hence, **IT IS RECOMMENDED TO RUN THE INSTALLATION SCRIPT IN A SCREEN SESSION**.  


## Check the Linux Headers
Our system misses the required Linux Headers, either due to kernel version is old, or the headers of the version is no longer available. So I simply changed the folder name of existing kernel headers whose version is close to the required one. It just works fine.
