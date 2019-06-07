# “Unmet dependencies” for kernel and other packages [duplicate]

## Problem
```
Reading package lists... Done
Building dependency tree       
Reading state information... Done
You might want to run 'apt-get -f install' to correct these:
The following packages have unmet dependencies:
 linux-image-extra-4.13.0-45-generic : Depends: linux-image-4.13.0-45-generic but it is not going to be installed
 linux-image-generic-hwe-16.04 : Depends: linux-image-4.15.0-43-generic but it is not going to be installed
 linux-modules-extra-4.15.0-38-generic : Depends: linux-image-4.15.0-38-generic but it is not going to be installed or
                                                  linux-image-unsigned-4.15.0-38-generic but it is not going to be installed
 linux-modules-extra-4.15.0-39-generic : Depends: linux-image-4.15.0-39-generic but it is not going to be installed or
                                                  linux-image-unsigned-4.15.0-39-generic but it is not going to be installed
 linux-modules-extra-4.15.0-43-generic : Depends: linux-image-4.15.0-43-generic but it is not going to be installed or
                                                  linux-image-unsigned-4.15.0-43-generic but it is not going to be installed
 ubuntu-cleaner : Depends: python-defer but it is not installable
                  Depends: python-lxml but it is not going to be installed
                  Depends: python-dbus but it is not going to be installed
                  Depends: python-aptdaemon but it is not installable
                  Depends: python-aptdaemon.gtk3widgets but it is not installable
E: Unmet dependencies. Try 'apt-get -f install' with no packages (or specify a solution).
```

UPD: result of - sudo apt-get -f install
```
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Correcting dependencies... Done
The following packages were automatically installed and are no longer required:
  dbconfig-common gyp javascript-common libaio1 libapr1 libaprutil1 libaprutil1-dbd-sqlite3 libaprutil1-ldap libevent-core-2.0-5 libjs-inherits libjs-jquery libjs-node-uuid libjs-sphinxdoc
  libjs-underscore liblua5.1-0 libmcrypt4 libssl-dev libssl-doc libuv1 libuv1-dev linux-headers-4.13.0-36 linux-headers-4.13.0-36-generic linux-headers-4.13.0-38 linux-headers-4.13.0-38-generic
  linux-headers-4.13.0-45 linux-headers-4.13.0-45-generic linux-headers-4.15.0-38 linux-headers-4.15.0-38-generic linux-headers-4.15.0-39 linux-headers-4.15.0-39-generic linux-image-4.13.0-36-generic
  linux-image-4.13.0-38-generic linux-image-4.13.0-45-generic linux-image-4.15.0-38-generic linux-image-4.15.0-39-generic linux-image-extra-4.13.0-36-generic linux-image-extra-4.13.0-38-generic
  linux-image-extra-4.13.0-45-generic linux-modules-4.15.0-38-generic linux-modules-4.15.0-39-generic linux-modules-extra-4.15.0-38-generic linux-modules-extra-4.15.0-39-generic python-pkg-resources
  zlib1g-dev
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
  linux-image-4.13.0-45-generic linux-image-4.15.0-38-generic linux-image-4.15.0-39-generic linux-image-4.15.0-43-generic linux-modules-4.15.0-39-generic linux-modules-4.15.0-43-generic
Suggested packages:
  fdutils linux-tools linux-hwe-tools
The following NEW packages will be installed:
  linux-image-4.13.0-45-generic linux-image-4.15.0-38-generic linux-image-4.15.0-39-generic linux-image-4.15.0-43-generic linux-modules-4.15.0-39-generic linux-modules-4.15.0-43-generic
0 upgraded, 6 newly installed, 0 to remove and 400 not upgraded.
18 not fully installed or removed.
Need to get 0 B/70,4 MB of archives.
After this operation, 228 MB of additional disk space will be used.
Do you want to continue? [Y/n] Y
(Reading database ... 465767 files and directories currently installed.)
Preparing to unpack .../linux-modules-4.15.0-43-generic_4.15.0-43.46~16.04.1_amd64.deb ...
Unpacking linux-modules-4.15.0-43-generic (4.15.0-43.46~16.04.1) ...
dpkg: error processing archive /var/cache/apt/archives/linux-modules-4.15.0-43-generic_4.15.0-43.46~16.04.1_amd64.deb (--unpack):
 cannot copy extracted data for './boot/System.map-4.15.0-43-generic' to '/boot/System.map-4.15.0-43-generic.dpkg-new': failed to write (No space left on device)
No apport report written because the error message indicates a disk full error
                                                                              dpkg-deb: error: subprocess paste was killed by signal (Broken pipe)
Preparing to unpack .../linux-image-4.15.0-43-generic_4.15.0-43.46~16.04.1_amd64.deb ...
Unpacking linux-image-4.15.0-43-generic (4.15.0-43.46~16.04.1) ...
dpkg: error processing archive /var/cache/apt/archives/linux-image-4.15.0-43-generic_4.15.0-43.46~16.04.1_amd64.deb (--unpack):
 cannot copy extracted data for './boot/vmlinuz-4.15.0-43-generic' to '/boot/vmlinuz-4.15.0-43-generic.dpkg-new': failed to write (No space left on device)
No apport report written because the error message indicates a disk full error
                                                                              dpkg-deb: error: subprocess paste was killed by signal (Broken pipe)
Preparing to unpack .../linux-image-4.13.0-45-generic_4.13.0-45.50~16.04.1_amd64.deb ...
Examining /etc/kernel/preinst.d/
run-parts: executing /etc/kernel/preinst.d/intel-microcode 4.13.0-45-generic /boot/vmlinuz-4.13.0-45-generic
Done.
Unpacking linux-image-4.13.0-45-generic (4.13.0-45.50~16.04.1) ...
dpkg: error processing archive /var/cache/apt/archives/linux-image-4.13.0-45-generic_4.13.0-45.50~16.04.1_amd64.deb (--unpack):
 cannot copy extracted data for './boot/vmlinuz-4.13.0-45-generic' to '/boot/vmlinuz-4.13.0-45-generic.dpkg-new': failed to write (No space left on device)
No apport report written because the error message indicates a disk full error
                                                                              dpkg-deb: error: subprocess paste was killed by signal (Broken pipe)
Examining /etc/kernel/postrm.d .
run-parts: executing /etc/kernel/postrm.d/initramfs-tools 4.13.0-45-generic /boot/vmlinuz-4.13.0-45-generic
run-parts: executing /etc/kernel/postrm.d/zz-update-grub 4.13.0-45-generic /boot/vmlinuz-4.13.0-45-generic
Preparing to unpack .../linux-image-4.15.0-38-generic_4.15.0-38.41~16.04.1_amd64.deb ...
Unpacking linux-image-4.15.0-38-generic (4.15.0-38.41~16.04.1) ...
dpkg: error processing archive /var/cache/apt/archives/linux-image-4.15.0-38-generic_4.15.0-38.41~16.04.1_amd64.deb (--unpack):
 cannot copy extracted data for './boot/vmlinuz-4.15.0-38-generic' to '/boot/vmlinuz-4.15.0-38-generic.dpkg-new': failed to write (No space left on device)
No apport report written because MaxReports is reached already
                                                              dpkg-deb: error: subprocess paste was killed by signal (Broken pipe)
Preparing to unpack .../linux-modules-4.15.0-39-generic_4.15.0-39.42~16.04.1_amd64.deb ...
Unpacking linux-modules-4.15.0-39-generic (4.15.0-39.42~16.04.1) ...
dpkg: error processing archive /var/cache/apt/archives/linux-modules-4.15.0-39-generic_4.15.0-39.42~16.04.1_amd64.deb (--unpack):
 cannot copy extracted data for './boot/System.map-4.15.0-39-generic' to '/boot/System.map-4.15.0-39-generic.dpkg-new': failed to write (No space left on device)
No apport report written because MaxReports is reached already
                                                              dpkg-deb: error: subprocess paste was killed by signal (Broken pipe)
Preparing to unpack .../linux-image-4.15.0-39-generic_4.15.0-39.42~16.04.1_amd64.deb ...
Unpacking linux-image-4.15.0-39-generic (4.15.0-39.42~16.04.1) ...
dpkg: error processing archive /var/cache/apt/archives/linux-image-4.15.0-39-generic_4.15.0-39.42~16.04.1_amd64.deb (--unpack):
 cannot copy extracted data for './boot/vmlinuz-4.15.0-39-generic' to '/boot/vmlinuz-4.15.0-39-generic.dpkg-new': failed to write (No space left on device)
No apport report written because MaxReports is reached already
                                                              dpkg-deb: error: subprocess paste was killed by signal (Broken pipe)
Errors were encountered while processing:
 /var/cache/apt/archives/linux-modules-4.15.0-43-generic_4.15.0-43.46~16.04.1_amd64.deb
 /var/cache/apt/archives/linux-image-4.15.0-43-generic_4.15.0-43.46~16.04.1_amd64.deb
 /var/cache/apt/archives/linux-image-4.13.0-45-generic_4.13.0-45.50~16.04.1_amd64.deb
 /var/cache/apt/archives/linux-image-4.15.0-38-generic_4.15.0-38.41~16.04.1_amd64.deb
 /var/cache/apt/archives/linux-modules-4.15.0-39-generic_4.15.0-39.42~16.04.1_amd64.deb
 /var/cache/apt/archives/linux-image-4.15.0-39-generic_4.15.0-39.42~16.04.1_amd64.deb
E: Sub-process /usr/bin/dpkg returned an error code (1)
```
## Solution

- If that is not possible and ```/boot``` is full, remove some config* files by
CAUTION: Do not remove any file related to the two latest kernels.

```sudo rm /boot/<file-name>``` of old kernels or even some vimlinuz* files.
- After you have solved the problem run in terminal.
```sudo apt autoremove```
- To make space in "/" partition.
- To remove unwanted pakages. Run in terminal.
```sudo apt autoremove```
- To remove all .deb file in apt cache. Run in terminal.
```sudo apt clean```
- To remove old config files. Run in terminal.
```dpkg -l | grep '^rc' | awk '{print $2}' | xargs sudo apt purge -y```
- To empty every trash. Run in terminal.
```
rm -rf /home/*/.local/share/Trash/*/** &> /dev/null
sudo rm -rf /root/.local/share/Trash/*/** &> /dev/null
```
- To remove extra kernels. Run in terminal.
 ```
 sudo apt purge $(dpkg -l|egrep 'linux-image-[0-9]|linux-headers-[0-9]'|awk '{print $3,$2}'|grep -v `uname -r|cut -f1,2 -d"-"`|sort -nr|tail -n +4|awk '{ print $2}')
 ```
- To remove orphaned packages. Run in terminal.
```
sudo apt install deborphan
sudo deborphan | xargs sudo apt -y remove --purge
```
Move some saved video audio or other personal data files from /home to some other partition.
