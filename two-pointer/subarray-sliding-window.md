### Review

Sliding window is a complex form of two pointer. Generally, subarray problem needs sliding window



### [713. Subarray Product Less Than K](https://leetcode.com/problems/subarray-product-less-than-k/)

#### idea

Only one thing to remember: given an array ` [...i,...,j...]` , then from index `i` to `j` , the total number of subarray in is `j-i+1`. This is the __key__ in many subarray problem which will greatly reduce time complexity. 

 #### code

```c++
class Solution {
public:
    int numSubarrayProductLessThanK(vector<int>& nums, int k) {
        int n = nums.size();
        int ans = 0; 
        int prod = 1; 
        for (int i=0, j=0; j<n; ++j) {
            prod *= nums[j];
            // slide window to make prod<k
            while (i <= j && prod >= k) prod /= nums[i++];
            ans += j-i+1;
        }
        return ans;
    }
};
```



