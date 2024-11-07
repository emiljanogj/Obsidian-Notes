- Tuning profiles are found in `/usr/lib/tuned` or `/etc/tuned`. If same profile exists on both directories, the 2nd one takes priority. Example of a `/etc/tuned/virtual-guest/tuned.conf` file is given below:
```
#  
# tuned configuration  
# 
[main]
summary=Optimize for running inside a virtual guest
include=throughput-performance

[sysctl]
# If a workload mostly uses anonymous memory and it hits this limit, the entire
# working set is buffered for I/O, and any more write buffering would require
# swapping, so it's time to throttle writes until I/O can catch up.  Workloads
# that mostly use file mappings may be able to use even higher values.
#
# The generator of dirty data starts writeback at this percentage (system default
# is 20%)
vm.dirty_ratio = 30

# Filesystem I/O is usually much more efficient than swapping, so try to keep
# swapping low.  It's usually safe to go even lower than this on systems with
# server-grade storage.
vm.swappiness = 30
```

- To identify the currently active tuning profile, we use the following command:
```
tuned-adm active
```
- To list all the profiles, we use the following command:
```
tuned-adm list
```
- To get more info about a profile, we use the following command:
```
tuned-adm profile_info {profile-name}
```

- To switch to a different profile, we use the following command:
```
tuned-adm profile {profile-name}
```

- To get a profile recommendation, we use the following command:
```
tuned-adm recommend
```

#### Influence Process Scheduling

The Completely Fair Scheduler(CFS) organizes processes that are awaiting CPU time into a binary search tree(BST), where the first item in the tree has the least amount of previously spent cumulative CPU time. In addition, the order of this binary tree, is also influenced by a niceness value assigned to each process, which ranges in [-20, 19]. 

#### Nice Value

- The following command list all the processes along with their niceness value and CLS scheduling algorithms, sorting by the niceness value in decreasing order:
```
ps axo pid,comm,nice,cls --sort=-nice
```

- By default, when we run a command, the niceness of the process is set to 0. If we want to set it to another value, we use the following command:
```
nice -n {command} &
```


