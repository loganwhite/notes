1. Find the culprit sshfs process:
`$ pgrep -lf sshf`
2. Kill it:
`$ kill -9 <pid_of_sshfs_process>`
3. sudo force unmount the "unavailable" directory:
`$ sudo umount -f <mounted_dir>`
4. Remount the now "available" directory with sshfs ... and then tomorrow morning go back to step 1.
