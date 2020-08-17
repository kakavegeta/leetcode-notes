### Review

Sometimes it is not easy to identify the possibility to apply binary search algorithm. There is one kind which is generally difficult. These problems usually give you an array, and two variables, say x and y, and there is a constraint between x and y. I even don't know what I am say, but you can sense it as you go through the problems I list below:

- 1283.[Find the Smallest Divisor Given a Threshold](https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/discuss/446376/javacpython-bianry-search/401806)
- 1231.[Divide Chocolate](https://leetcode.com/problems/divide-chocolate/discuss/408503/Python-Binary-Search)
- 1011.[Capacity To Ship Packages In N Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/discuss/256729/javacpython-binary-search/351188?page=3)
- 875.[Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/discuss/152324/C++JavaPython-Binary-Search)
- 774.[Minimize Max Distance to Gas Station](https://leetcode.com/problems/minimize-max-distance-to-gas-station/discuss/113633/Easy-and-Concise-Solution-using-Binary-Search-C++JavaPython)
- 410.[Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/)

You need to create a function as new checker, instead of simply comparing mid and target. 

Also, you need to figure out what you should calculate, left_bound or right_bound.

As you figure out a template, those problem might become easier:

a. create a function



### [875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/discuss/152324/C++JavaPython-Binary-Search)

```c++
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int H) {
        int lo = 1;
        int hi;
        for (auto& pile: piles) hi = max(hi, pile);
        
        // left bound
        while (lo <= hi) {
            int mid = lo + (hi-lo)/2;
            if (canEatup(mid, piles, H)) hi = mid-1;
            else lo = mid+1;
        }
        return lo;
    }
    
    bool canEatup(int speed, vector<int>& piles, int H) {
        int h = 0;
        for (auto& pile: piles) {
            h += pile/speed + (pile%speed==0?0:1);
        }
        if (h<=H) return true;
        return false;
    }
};
```



### [410. Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/)

Data type should be noticed.

```c++
class Solution {
public:
    int splitArray(vector<int>& nums, int m) {
        long lo = 0, hi = 0;
        for(auto num: nums) {
           lo = max(lo, (long)num); 
           hi += num;
        }
        // left bound
        while (lo <= hi) {
            long mid = lo + (hi-lo)/2;
            if (isValid(mid, m, nums)) hi = mid-1;
            else lo = mid+1;
        }
        return lo;
        
    }
    
    bool isValid(int maxSum, int m, vector<int>& nums) {
        long sum = 0, cnt = 1;
        for (int i=0; i<nums.size(); ++i) {
            sum += nums[i];
            if (sum > maxSum) {
                sum = nums[i];
                cnt++;
                if (cnt > m) return false;
            } 
        }
        return true;
    }
};
```

