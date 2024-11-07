**Top-Down**
```C++
for(int k=i; k<=j; j++)
	result = max(result, topDown(nums, i, k-1) + (i-1 >= 0 ? nums[i-1] : 1) * nums[k] * (j < nums.size() - 1 ? nums[j+1] : 1) + topDown(nums, k+1, j));
```


**Bottom-Up**
```C++
for(int l=1; l<n; l++){
	for(int i=0; i<n-l; i++){
		int j = i+l;
		for(int k=i; k<=j; k++){
			dp[i][j] = max(dp[i][j], ((k > i && k > 0) ? dp[i][k-1] : 0) + (i > 0 ? nums[i-1] : 1) * nums[k] * (j < nums.size() - 1 ? nums[j+1] : 1) + (k < j) ? dp[k][j] : 0);
		}
	}
}
```