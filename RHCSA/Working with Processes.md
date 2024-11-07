
- To list processes that belong to a certain user, we use the following command:
```
pgrep -l -u {user}
```

- To kill all processes that belong to a certain user we use the following command:
```
pkill -SIGKILL -u {user}
```

- To terminate several processes based on their command name we use the following command:
```
killall control
```
- To list background jobs, we use the following command:
```
jobs
```

- To terminate a background job, we use the following command:
```
kill -SIGTERM %{job-number}
```

- To list all the current user on the system, we use the following command:
```
w
```

An example of the output of the above command is as follows:
![[Pasted image 20230607001841.png]]

Note that all the user login sessions are associated with a terminal session (ttyN) or a pseudo-terminal session pts/N in case of a graphical window or a remote login session.


#### Monitor Process Activity