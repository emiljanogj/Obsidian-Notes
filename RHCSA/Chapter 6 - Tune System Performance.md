![[`Pasted image 20240110222224.png]]`

- To list all the processes that belong to a certain user we use the following command:
```
pgrep -l -u {username}
```
- To kill these processes we use the command:
```
pkill -SIGKILL -u {username}
```
- To kill processes with a specific terminal ID we use the following command:
```
pkill -SIGKILL -t tty3
```
- To kill all the processes with a given parent UUID we use the following command:
```
pkill -SIGKILL -P {parent-uuid}
```
- To kill the processes that were initiated from a certain command we use the following:
```
killall {command}
```
- To stop the first job that gets outputted when we run `jobs` we use the following command:
```
kill -SIGSTOP %1
```


### Tuning Profiles
- To view the active tuning profile we use the command:
```
tuned-adm active
```
- To list all the profiles we use the command:
```
tuned-adm list
```
- To select an active profile we use the command:
```
tuned-adm profile {profile-name}
```
- The settings for a given profile name can be found in `/usr/lib/tuned/{profile-name}/tuned.conf` or we can use the command:
```
tuned-adm profile_info
```

### Influence Process Scheduling

- System processes are scheduled based on the priorities and queues
- Normal processes use the **Completely Fair Scheduler(CFS)**, which doesn't use the priorities, but organizes the processes in a time-weighted binary tree, with the first item having the least cumulative CPU time.
- The binary tree is also influenced by the niceness value which ranges from -20(increased priority) to 19(decreased priority).
- The real time priority is equal to `nice_value + 20`
- To print the command + nice value of a process we use the following command:
```
ps -o pid,comm,nice {pid}
```
- The `nice` command starts a command as a background job with the default nice value(10)
- To start it with another nice value we use the `nice -n {nice-value}` command and to change the nice value we use the `renice -n {nice-value}` command
- To get the number of CPU cores we use the command `grep -c '^processor' /proc/cpuinfo` where the `-c` option specifies that we only need the count. 
- To kill all the processes that were spawned by a command we use the command `pkill {command}`
- Useful command:
```
ps -o pid,pcpu,nice,comm $(pgrep sha1sum)
```
- To sort processes by CPU usage we use the following command:
```
ps -aux --sort=pcpu
```
