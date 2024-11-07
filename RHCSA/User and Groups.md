#### Difference between `su -` and `su`

- The command `su` starts a non-login shell, while the command `su - `(with the dash option) starts a login shell. The main distinction between the two commands is that `su -` sets up the shell environment as if it is a new login as that user, while `su` starts a shell as that user, but uses the original user's environment settings.

- To add a user to the `sudoers` group we use the following command:
```
sudo usermod -L {username}
```
- Assume we have the following line in the `/etc/sudoers` group:
```
 %wheel        ALL=(ALL:ALL)       ALL
```
This line specifies that in ALL systems that have this file(1st ALL), users of this group are allowed to run ALL commands(4th ALL) as ALL the users(2nd ALL) and ALL the groups in the system(3rd ALL).
- To enable sudo access for `user01` we add the following file in `/etc/sudoers.d`
```
user01         ALL=(ALL)            ALL
```
To allow this user to run the commands without a password we use the `NOPASSWD` keyword before the last ALL.

- To enable the users in the `games` group to run the `/bin/id` command as the `operator` user we use the following command:
```
%games        ALL=(operator)        /bin/id
```

