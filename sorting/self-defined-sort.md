### Review

Sometimes questions might define another comparison scheme to sort which is different from numerical comparison the most common one we use. In python3, since `cmp` have already been removed,  you need to use `cmp_to_key` to transform self defined scheme to `key`. 

### [179. Largest Number](https://leetcode.com/problems/largest-number/)

#### idea

Initially I come to radix sort... But after 30 minutes I realized it's too complex... Then I click discussion... Damn it, the devil here is not sorting method but comparison schema! For example, we need to put `9` before `20`. But how? The essence is `a-b>b-a`! Amazing!

#### code

```python
#Python
class Solution:
    
    def cmp(self, a, b):
        if a+b < b+a: return 1
        elif a+b > b+a: return -1
        else: return 0
    
    
    def largestNumber(self, nums: List[int]) -> str:
        #return ''.join(sorted(map(str, nums), key = lambda a, b: a+b > b+a))
        ans = "".join(sorted(map(str, nums), key = cmp_to_key(self.cmp)))
        return ans.lstrip("0") or "0";
```

```c++
//CPP
class Solution {
public:
    string largestNumber(vector<int>& nums) {
        sort(nums.begin(), nums.end(), [](int a, int b){
            return to_string(a) + to_string(b) > to_string(b) + to_string(a);
        });
        
        string ans;
        for (int n: nums) ans += to_string(n);
        return ans[0] == '0'? "0": ans;
    }
};
```

