### [926. Flip String to Monotone Increasing](https://leetcode.com/problems/flip-string-to-monotone-increasing/)

A very Interesting one! We can use dynamic programming or prefix sum~



```c++
// DP
class Solution {
public:
    int minFlipsMonoIncr(string s) {
        int n = s.size();
        int flips = 0;
        int ones = 0;
        for (int i=0; i<n; ++i) {
            if (s[i] == '0') flips++;
            else ones++;
            flips = min(flips, ones); 
        }
        return flips;
    }
};
```



