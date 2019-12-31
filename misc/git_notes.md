## How to forcibly pull the remote branch. discard the local changes and use the remote one.
```bash
git fetch --all
git reset --hard origin/<branch-name>
```
**Explanation:**

`git fetch` downloads the latest from remote without trying to `merge` or `rebase` anything.

Then the git reset resets the branch to what you just fetched. The `--hard` option changes all the files in your working tree to match the files in `origin/master`.
