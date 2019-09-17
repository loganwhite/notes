To install python-igraph use: 
```bash
sudo apt-get install libxml2-dev
sudo pip install python-igraph
```
Go to the Gurobi Optimizer download page http://www.gurobi.com/downloads/gurobi-optimizer and download the latest version of Gurobi Optimizer

Download the tar file. Unzip the file with:
```bash
tar -zFor shared use, put the folder into /opt
```
Add the following environment variables to your BASH
```bash
export GUROBI_HOME="/opt/gurobi810/linux64" 
export PATH="${PATH}:${GUROBI_HOME}/bin" 
export LD_LIBRARY_PATH="${LD_LIBRARY_PATH}:${GUROBI_HOME}/lib"
```
Go to https://user.gurobi.com/download/licenses/free-academic to get the free academic key. And follow the instructions.

Type `gurobi.sh`  to test the installation.

Go to `<installdir>` and execute: （MAC /Library/gurobi461）
```bash
python setup.py install
xvf ***.tar.gz
```
