### Review

### [838. Push Dominos](https://leetcode.com/problems/push-dominoes/)

```c++
class Solution {
public:
    string pushDominoes(string ds) {
        ds = 'L'+ds+'R';
        int n = ds.size();
        string ans;
        for (int i=0, j=1; j<n; ++j) {
            if (ds[j] == '.') continue;
            if (i > 0) ans += ds[i];
            int mid = j-i-1;
            if (ds[i] == ds[j]) ans += string(mid, ds[i]);
            else if (ds[i] == 'L' && ds[j] == 'R') ans += string(mid, '.');
            else ans += string(mid/2, 'R') + string(mid%2, '.') + string(mid/2, 'L');
            i = j;
        }
        return ans;
            
    }
};
```

