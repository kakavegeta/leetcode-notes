### [1546. Maximum Number of Non-Overlapping Subarrays With Sum Equals Target](https://leetcode.com/problems/maximum-number-of-non-overlapping-subarrays-with-sum-equals-target/)

```c++
// dp
class Solution {
public:
    int maxNonOverlapping(vector<int>& nums, int target) {
        unordered_map<int, int> ht;
        ht[0] = 0;
        int cursum = 0, ans = 0;
        
        for (int i=0; i<nums.size(); ++i) {
            cursum += nums[i];
            if (ht.count(cursum-target)) {
                ans = max(ans, ht[cursum-target]+1);
            }    
            ht[cursum] = ans;
        }
        return ans;
    }
};

// record pre index to prevent overlapping
class Solution {
public:
    int maxNonOverlapping(vector<int>& nums, int target) {
        unordered_map<int, int> ht;
        ht[0] = -1;
        int cursum = 0, ans = 0;
        int idx = -1;
        
        for (int i=0; i<nums.size(); ++i) {
            cursum += nums[i];
            if (ht.count(cursum-target)) {
                if (ht[cursum-target] >= idx) {
                    ans++; idx = i;
                }
            }
            ht[cursum] = i;
        }
        return ans;
    }
};

```

