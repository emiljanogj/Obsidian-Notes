```C++
class Solution {
public:
    vector<int> sieve(int M) {
        vector<int> spf(M+1);
        iota(begin(spf), end(spf), 0);
        for(int i = 2; i*i < M; i++) 
            if(spf[i] == i) 
                for(int j = i*i; j < M; j += i)
                    if(spf[j] == j)
                        spf[j] = i;            
        return spf;
    }
    void getPrimes(int x, vector<int>& spf, set<int>& primes) {
        while (x != 1) {
            int p = spf[x];
            // cout<<"x = "<<x<<" p = "<<p<<endl;
            primes.insert(p);
            while(x % p == 0) x /= p;
        }
    }
    int distinctPrimeFactors(vector<int>& nums){
        set<int> res;
        
        vector<int> spf = sieve(*max_element(nums.begin(), nums.end()) + 1);

        for(int n: nums)
            getPrimes(n, spf, res);

        return res.size();      
    }
};
```