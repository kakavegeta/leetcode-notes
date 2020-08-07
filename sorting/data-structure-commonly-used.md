### Review

`multiset` and `map` in c++ are ordered

### [220. Contains Duplicate III](https://leetcode.com/problems/contains-duplicate-iii/)

#### multiset

```c++
class Solution {
public:
    bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
        multiset<long> ordered;
        int n = nums.size(); 
        int j = 0;
        for (int i=0; i<n; ++i) {
            if (i-j>k) ordered.erase(ordered.find(nums[j++]));
            auto it = ordered.insert(nums[i]);
            if (it != ordered.begin() && *it - *prev(it) <= t) return true;
            if (next(it) != ordered.end() && *next(it) - *it <= t) return true;
        }
        return false;     
    }
};
```

```c++
class Solution {
public:
    bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
        multiset<long> ordered;
        int n = nums.size(); 
        int j = 0;
        for (int i=0; i<n; ++i) {
            if (i-j>k) ordered.erase(ordered.find(nums[j++]));
            
            auto it = ordered.lower_bound((long)nums[i]-t);
            if (it != ordered.end() && abs(*it-nums[i])<=t ) return true;
            // must insert nums[i] at the end. If not, *it might be nums[i], that is, it will always be 
            // successful to find lower_bound()
            ordered.insert(nums[i]);
        }
        return false;  
    }
};
```

#### bucket sort

```

```

