## How to forcibly pull the remote branch. discard the local changes and use the remote one.
```bash
git fetch --all
git reset --hard origin/<branch-name>
```
**Explanation:**

`git fetch` downloads the latest from remote without trying to `merge` or `rebase` anything.

Then the git reset resets the branch to what you just fetched. The `--hard` option changes all the files in your working tree to match the files in `origin/master`.


## How to push large files to Github?
Github has an restriction that each file should be less than 100MB. Otherwise, git large file system should be used. To use this, the following steps should be applied.
1. Install the git-lfs plugin.
Visit https://git-lfs.com and choose a suitable means to install the plugin on your system.
2. Apply lfs to your local repository.
Go the the repository foler and install `lfs` for your repo using the following command
```bash
git lfs install
```
Then, you will need to track the large files. For example, track the large file with suffix "*.psd".
```bash
git lfs track "*.psd"
```
After running the command, the `.gitattributes` is created, and this file should be staged by the git repo. Hence,
```bash
git add .gitattributes
```
Finally, push the repo to the Github remote repo as usual. 
