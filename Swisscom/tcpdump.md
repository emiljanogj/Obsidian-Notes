- Example Usage:
```
tcpdump -i enp2s0 -nnvvpp host 10.123.130.16
```

- To filter on both host and port we use the following command:
```bash
tcpdump -nnvvpp host 10.144.35.8 and port 22
```
