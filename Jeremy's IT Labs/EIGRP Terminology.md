Several terms to keep in mind:
- **Feasible Successor** - Advertised distance is lower than the successor's route feasible distance
- 2 types of distances:
	- *Feasible distance* - The metric of the best route to reach a network
	- *Reported distance* - The metric advertised by a neighbouring router for a specific route
- **EIGRP** will perform unequal-cost load-balancing over feasible successor routes
- Default K-values - EIGRP will compute the cost of the path using several metrics as follows:
	- K1 - Bandwidth(1)
	- K2 - Load(0)
	- K3 - Delay(1)
	- K4 - Reliability(0)
	- K5 - MTU(0)
*Note*: By default, only the bandwidth and the delay are used so they are set to 0 and the other values are set to 1