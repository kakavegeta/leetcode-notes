### Review

As you see partition in quicksort, you can catch shadow of application of two pointers, either Lumo scheme or Hoare scheme. In one pass O(n), set three index `low` , `high`, and `cur`, through swapping, all elements before `cur` are sorted. 



### [75. Sort Colors](https://leetcode.com/problems/sort-colors/)

#### idea

An easy but illuminating question~ Counting sort is ok, but simply apply partition scheme will only need one pass.

#### code

```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int left=0, right=nums.size()-1;
        
        for (int i=0; i<=right; ++i) {
            if (nums[i] == 0) swap(nums[i], nums[left++]);
            // one place to notice. If nums[i]==2, if not i--, we will 
            // miss the new element newly swapped in i by i++. If nums[i]==0
            // we do not need to care about that because we've already scaned that element
            if (nums[i] == 2) swap(nums[i--], nums[right--]);
        }
    }
};
```

```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int left=0, right=nums.size()-1;
        for(int i=0; i<=right; ++i){
            // another way. But nums[i]==2 must comes first. The reason is same
            // with why i-- in previous solution. We scan from left to right, if 
            // we do not check nums[i]==0 at last, we will miss newly swapped element.
            while(nums[i]==2 && i<right) swap(nums[right--], nums[i]);
            while(nums[i]==0 && i>left) swap(nums[left++], nums[i]);
        }
    }
};
```



