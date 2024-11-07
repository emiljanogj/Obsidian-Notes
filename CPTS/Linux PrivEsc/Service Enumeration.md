#Linux_PrivilegeEscalation 

- To find history files in Linux we use the following command:
```bash
find / -type f \( -name *_hist -o -name *_history \) -exec ls -l {} \; 2>/dev/null
```

- To get a list of installed packages we use the following command:
```bash
apt list --installed | tr "/" " " | cut -d" " -f1,3 | sed 's/[0-9]://g' | tee -a installed_pkgs.list
```

- To check our binaries against **GTFObins** we use the following command:
```bash
for i in $(curl -s https://gtfobins.github.io/ | html2text | cut -d" " -f1 | sed '/^[[:space:]]*$/d');do if grep -q "$i" installed_pkgs.list;then echo "Check GTFO for: $i";fi;done
```

- To find all the configuration files we use the following command:
```bash
find / -type f \( -name *.conf -o -name *.config \) -exec ls -l {} \; 2>/dev/null
```

**Notes on the above command**
- `\( -name *.conf -o -name *.config \)` - -o is the logical or operator
- `-exec ls -l {} \;` - used to execute a command on the found files. `\;` marks the end of the exec command




