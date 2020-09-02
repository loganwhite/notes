## Simple Code Template

Some times, running experiements requires shifting parameters in the source code and recompile again and again. This is tedious but has a specific pattern.

We want to automate this procedure with programs. The simpliest way is to create a code template and use substitution softwares (e.g., `sed`) to replace the "placeholders" with needed parameters.

For example, we want to replace all "xxx"s in the `profiles/64B_base.yaml` source file with the value of shell variable `${i}` and create a new file named `profiles/64B_${i}.yaml`, we can use the following code.
```bash
sed "s/xxx/${i}/g" profiles/64B_base.yaml >> profiles/64B_${i}.yaml
```


## Remote process execution

Network experiments often involving cooperations between multiple end hosts, we can simply use `ssh` to execute processes on remote devices.
```bash
ssh root@server "/home/user/mywork/tests/process.sh"
```
