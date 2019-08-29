1. Find the culprit sshfs process:
`$ pgrep -lf sshf`
2. Kill it:
`$ kill -9 <pid_of_sshfs_process>`
3. sudo force unmount the "unavailable" directory:
`$ sudo umount -f <mounted_dir>`
4. Remount the now "available" directory with sshfs ... and then tomorrow morning go back to step 1.


# `sshfs` cannot mount remote file system. `kext load failed: -603946985  mount_osxfuse: the file system is not available (255)`
FUSE needs to register a virtual device for exchanging messages between the kernel and the actual file system implementation running in user space. The number of available device slots is limited by macOS. So if you are using other software like VMware, VirtualBox, TunTap, Intel HAXM, ..., that eat up all free device slots, FUSE will not be able to register its virtual device.

On a brand new install of macOS 10.12 there are six free device slots. VMware and VirtualBox require multiple slots each. TunTap (VPN) requires two slots, as far as I know. Intel HAXM requires at least one slot.

(In my machine, because I have installed android SDK and the Intel HAXM is running.)

Run
```bash
Kextstat
```
to list your kernel extensions on your system.

Then, unmount an extension.
```bash
sudo kextunload -b <extension>
```
