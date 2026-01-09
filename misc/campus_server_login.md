```bash
#!/bin/bash

acc=$1
passwd=$2

# 1. Capture the IP address first for cleaner code
# This looks for an IP starting with 10.x.x.x
ip_addr=$(ifconfig | grep "inet 10" | awk '{print $2}' | head -n 1)

# 2. Execute curl with double quotes to allow variable expansion
curl "https://lan.bistu.edu.cn:802/eportal/portal/login?callback=dr1004&login_method=1&user_account=%2C0%2C${acc}&user_password=${passwd}&wlan_user_ip=${ip_addr}"
```
How to use it

Save the file (e.g., `login.sh`).

Give it execute permissions: `chmod +x login.sh`.

Run it: `./login.sh your_username your_password`.
