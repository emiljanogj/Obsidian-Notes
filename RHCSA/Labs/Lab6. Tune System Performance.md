- To list processes based on cpu usage we use the following command:
```
ps -aux --sort=pcpu
```
- To check the nice value of a process spawned from a given command we use the following command:
```
ps -o pid,nice $(pgrep {command})
```

