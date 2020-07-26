### Review

For details, see [Partial sorting](https://en.wikipedia.org/wiki/Partial_sorting). Top K problem is  super frequent based on interviewing stories.



### [1471. The k Strongest Values in an Array](https://leetcode.com/problems/the-k-strongest-values-in-an-array/)

#### idea

If the question ask resulting array ordered, then no chance we can apply randomized algorithms here.  To solve this problem, there are two steps: find median and then find k strongest values. To find median, we do not need to sort whereas partition can simply work in O(n), and the pivot index is (n+1)/2.  Then in the second step, similar operation but with different pivot index as k and different comparison rule defined by "stronger value".

#### code

```c++
class Solution {
public:
    vector<int> getStrongest(vector<int>& arr, int k) {
        int n = arr.size();
        nth_element(arr.begin(), arr.begin() + (n-1)/2, arr.end());
        int med = arr[(n-1)/2];
        auto compare = [&med](int &a, int &b) {
            return (abs(a-med) > abs(b-med) || (abs(a-med) == abs(b-med) && a > b));
        };
        
        nth_element(arr.begin(), arr.begin()+k, arr.end(), compare);
        arr.erase(arr.begin()+k, arr.end());
        return arr;
    }
};
```

