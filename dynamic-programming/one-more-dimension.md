### Review

Suppose our `dp` matrix can be built with dimension `n*n`, generally speaking you can fill your `dp` matrix with `for (i=0; i<n; ++i) {for (j=0; j<n; ++j) {...}}`. However, in some problem you might need to add one more level of loop: `for (i=0; i<n; ++i) { for (k = 0; k<K; ++k) {for (j=0; j<n; ++j) {...}}}`.  Several years ago such problem might be in hard level, but given recent problem published, they are medium. To some extent, the difficulty of leetcode is increasing. 



### [1043. Partition Array for Maximum Sum](https://leetcode.com/problems/partition-array-for-maximum-sum/)

#### idea

Given K which is the length of partition at most, make `dp[i]` as the maximum sum from A[0]~A[i]. At `i`, we can do `K` different partitions. Just find max `dp[i]`  in all these K partition.

#### code

```c++
class Solution {
public:
    int maxSumAfterPartitioning(vector<int>& A, int K) {
        int n = A.size();
        vector<int> dp(n+1, 0);
        for (int i=1; i<=n; ++i) {
            int curmax = 0;
            for (int k=1; k <= min(i, K); ++k) {
                curmax = max(curmax, A[i-k]);
                dp[i] = max(dp[i], dp[i-k]+k*curmax);
            }
        }
        return dp[n];
    }
};
```















