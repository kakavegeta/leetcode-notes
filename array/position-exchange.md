### [41. First Missing Positive](https://leetcode.com/problems/first-missing-positive/)

#### idea

If no restriction of constant space, we can simply use a set or hash table to find the first missing positive. However, we should run in O(n) and constant space. Now things become tough. I do not know why someone come up with such excellent solution, but just remember it and interpret the process might be helpful. The key insight here is: we put element (1-index based) in its "correct" place (0-index based). For example, we have [3, 4, -1, 1], we need to put 3 to index 2, 4 to index 3, 1 to index 0. For element >n and <=0, we ignore them. 

Actually, this approach use the original array as a hash table since we are not allowed to use extra space.  Imagine that only an infinite array [1,2,3,4,5,6...,+inf] can have no missing positive. Now we have an array with given length n, so we want to build a hash table looks like ` {0:1, 1:2, 2:3, 3:4,..., n-1:n}`. We iterate i from 0 to n-1, if we find a mismatch, then the first missing positive should be i+1. Take [3, 4, -1, 1] as example, we want to build `{0:1, 1:-1, 2:3, 3:4}` , and obviously the first missing positive is 2. But how can we build such a "hash table"? See the comments in code~

#### code

```c++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n = nums.size(); 
        for (int i = 0; i<n; ++i) {
            // most significant part in this approach. For each index i and nums[i], 
            // we want to put nums[i] to position nums[i]-1.
            // therefore, we use a while loop to achieve this until 1) nums[i]<=0, or 2) nums[i]>=n or, 
            // 3) nums[i] in its 'correct' position 
            while (nums[i]>0 && nums[i] <= n && nums[i] != nums[nums[i]-1]) {
                swap(nums[nums[i]-1], nums[i]);
            }
        }        
        for (int i=0; i<n; ++i) {
            if (nums[i] != i+1) return i+1;
        }
        return n+1;
    }
};
```



