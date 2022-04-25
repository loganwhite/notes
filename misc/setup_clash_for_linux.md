## Download the clash binary from Github.
Visit https://github.com/Dreamacro/clash/releases, and then execute the following commands.
```bash
$ gunzip clash-linux-amd64-v0.18.0.gz
$ sudo mv clash-linux-amd64-v1.4.2 /usr/local/bin/clash
$ sudo chmod +x /usr/local/bin/clash
$ clash
```
After executing the Clash binary, configuration file will be generated in the home folder of the current user at `$HOME/.config/clash`.

## Setup the configuration file.
Add the following content into your `config.yaml` file.
```yaml
mixed-port: 7890
socks-port: 7891

external-controller: '0.0.0.0:9090'
secret: 'xxx'

proxies:
  - name: [some name]
    type: ss
    server: [some server host ]
    port: [some port]
    ...
```
Then, we can move the `$HOME/.config/clash` folder to `/etc/` to make it a global configuration file.
```bash
sudo mv ~/.config/clash /etc
```
Then, add&edit&save a file as `sudo vi /etc/systemd/system/clash.service`, and add the following content.
```
[Unit]
Description=Clash Daemon

[Service]
User=root
ExecStart=/usr/local/bin/clash -d /etc/clash/
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
Then, enable and start the service
```bash
sudo systemctl enable clash.service
sudo systemctl start clash.service
```
Now, the clash service is enabled and started. We can go to the external controller to configure the server.
## External controller
Visit  http://clash.razord.top/#/proxies, and set the Host,Port,Secret as you have configured in the `config.yaml` file.
Then, you can choose the proxy servers in the GUI.

## Setup linux system proxies.
```bash
sudo vim /etc/environment
```
Add the following content
```bash
export http_proxy=127.0.0.1:7890
export https_proxy=127.0.0.1:7890
export socks_proxy=127.0.0.1:7891
```
Then, setup apt proxy because otherwise apt will fail to get software packages.
```bash
sudo vim /etc/apt/apt.conf.d/proxy.conf
```
Then, add the following content 
```conf
Acquire {
  HTTP::proxy "http://127.0.0.1:7890";
  HTTPS::proxy "http://127.0.0.1:7890";
}
```
Then, you are done.
