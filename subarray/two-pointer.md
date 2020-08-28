### [845. Longest Mountain in Array](https://leetcode.com/problems/longest-mountain-in-array/)

DP

Two passes. One from front to end, the other from end to front.  

```c++
class Solution {
public:
    int longestMountain(vector<int>& A) {
        // two pass
        int n = A.size();
        vector<int> downs(n, 0);
        vector<int> ups(n, 0);
        for (int i=n-2; i>=0; --i) if (A[i] > A[i+1]) downs[i] = downs[i+1]+1;
        
        for (int i=1; i<n; ++i) if (A[i] > A[i-1]) ups[i] = ups[i-1]+1;
        
        int ans = 0;
        for (int i=0; i<n; ++i) {
            //cout << ups[i] << " " << downs[i] << endl;
            if (ups[i]==0 || downs[i] == 0) continue;
            ans = max(ups[i]+downs[i]+1, ans); 
        }
        
        return ans;        
    }
};
```

