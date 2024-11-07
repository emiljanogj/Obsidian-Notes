#CPTS

---

## /bin/sh -i

```shell-session
/bin/sh -i
sh: no job control in this shell
sh-4.2$
```

## Perl

```bash
perl -e 'exec "/bin/sh";'
```

or from a script:

```bash
perl: exec "/bin/sh"
```


## AWK

```bash
awk 'BEGIN {system("/bin/sh")}'
```

## Find + Exec

```bash

find . -exec /bin/sh \; -quit
```

```bash
find . -exec /bin/awk 'BEGIN {system("/bin/sh")}' \; -quit
```

## VIM

```bash
vim -c ':!/bin/sh'
```

```bash
vim
:set shell=/bin/sh
:shell
```

**Note**: In any case, to run one of the above commands, we need appropriate permissions. To check permissions for our user we use the following command:
```bash
sudo -l
```





