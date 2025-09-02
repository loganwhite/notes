# Prerequisition (local desktop operations)

*Notice: Skip this step if your conputer has XQuartz installed!*

Go to https://www.xquartz.org and download the latest version of XQuartz software, which is an X window software suit for MacOS.

# Remote Connection (local desktop operations)
1. connect the VPN with EasyConnect (ask le'von for msg auth code)
2. connect to the server with `ssh` with `-Y` option enabled.
```bash
ssh -Y arya@10.199.201.19
```

# Execute the GUI program (Server side operation accessed by `ssh` in the terminal)
Go to the folder of the GUI program on the server and execute the program.
For example, when executing FAE, execute the following script
```bash
cd ~/FAE
conda activate fae
python MainFrameCall.py
```

# Operate the server GUI program on your local machine (local desktop operations)
The GUI program should now be running on the server side but with its GUI forwarded to the local machine. You could use the remote GUI program as if it is on your local computer. 

# Post message
This method is applicable to almost all GUI software running on a remote server that does not have an desktop environment, or the server is unable to establish a GUI-based remote access.
