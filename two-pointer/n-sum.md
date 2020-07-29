### Review

2sum and 3sum are classic problems to apply two pointer trick, espicially 3sum. Keys to remember: to employ two pointer trick, you have to sort the array first because only after sorting we can do comparison between elements or sum of elements by index (pointer). Random array has no sense to use two pointer.



### [15. 3Sum](https://leetcode.com/problems/3sum/)

#### idea

One difficulty in this problem is duplicates. Remember to skip duplicates. 

Time: O(nlogn + n*n)

#### code

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        int lo, hi;
        vector<vector<int>> ans;
        // i is the fixed number, move lo and hi to find desired tuple.  
        for (int i=0; i<n-2; ++i) {
            if (nums[i]>0) break;
            while (i>0 && i<n && nums[i]==nums[i-1]) i++;
            lo = i+1, hi = n-1;
            while (lo < hi){
                if (nums[i]+nums[lo]+nums[hi] == 0) {
                   ans.push_back({nums[i], nums[lo++], nums[hi--]});
                   // skip duplicates
                   while (lo < hi && nums[lo] == nums[lo-1]) lo++;
                   while (lo < hi && nums[hi] == nums[hi+1]) hi--;
                }
                // of course you can skip in condition >0 or <0. But ignore here.
                else if (nums[i]+nums[lo]+nums[hi] > 0) hi--;
                else lo++;
            }

        }
        return ans;
    }
};
```



