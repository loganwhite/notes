At the end of the `/etc/sudoers` file add this line:
```bash
username     ALL=(ALL) NOPASSWD:ALL
```
Replace username with your account username, of course. Save the file and exit with <ESC>wq. If you have any sort of syntax problem, visudo will warn you and you can abort the change or open the file for editing again.

Note that if the file is not writable, `chmod` should be used to grant the write permission.
```bash
chmod +w /etc/sudoers
```
