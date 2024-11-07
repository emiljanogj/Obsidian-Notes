
- Cost = Reference Bandwidth / Interface Bandwidth where Reference Bandwidth = 100 Mbps 
- For interfaces > 100 Mbps, cost = 1 as cost cannot be < 1, however this is not desired
- This problem can be remedied by changing the reference bandwidth as follows
```
router ospf 1
auto-cost reference-bandwidth 100000
```
- OSPF will then select the route with the smallest cost. However, we can manually manipulate this by changing the bandwidth or the cost. Changing the cost is desirable since it doesn't affect other properties. Cost can be changed as follows:
```
interface FastEthernet 0.0
ip ospf cost 50
show ip ospf interface FastEthernet 0/0
```


