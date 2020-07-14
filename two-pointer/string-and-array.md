### Review

Problems that require you modify string/array in specific conditions might apply to two pointer trick. You can set <i, j> as begin-begin pointers, or begin-end pointers, to iterate through all elements in string/array. In loop, say `j` is responsible to scan every element, and `i` is responsible to check conditions and do modification. It is easy to digest the idea through some problems.



### [1209. Remove All Adjacent Duplicates in String II](https://leetcode.com/problems/next-greater-node-in-linked-list/) 

#### idea

Initially set `i=0, j=0`, `j` scans all elements, and `i` checks whether there are`k` duplicates, if yes, `i-=k`, and in next loop, set `s[i]=s[j]` to modify string in place. As result, all characters before `i+1` will be the required answer.

#### code

```c++
class Solution {
public:
    string removeDuplicates(string s, int k) {
        int i, j;
        int n = s.size();
        vector<int> cnt(n, 0);
        
        for (i=0, j=0; j<n; ++j, ++i) {
            s[i] = s[j];
            // record every elememt's frequency
            if (i > 0 and s[i] == s[i-1]) cnt[i] = cnt[i-1]+1;
            else cnt[i] = 1;
            // if there are k adjacent duplicates, move i to position
            // before the first of duplicates
            if (cnt[i] == k) i -= k;
        }
        return s.substr(0, i);
    }
};
```



### [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

#### idea

The basic idea is simple: for position `i`, find its left-greatest and right-greatest, then pick the smaller of the two as `h`, then `h - height[i]` is the greatest volume `i` can store. Sum up each position's greatest volume.

```c++
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size();
        if (n==0) return 0; 
        
        int l = 0, r = n-1;
        int lmx = height[l], rmx = height[r]; 
        int ans = 0; 
        while (l < r) {
            if (lmx < rmx) {
                ans += lmx - height[l++];
                lmx = max(lmx, height[l]);
            } else {
                ans += rmx - height[r--];
                rmx = max(rmx, height[r]);
            }       
        }
        return ans;
    }
};
```

