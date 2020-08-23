### [316. Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/)

```c++
class Solution {
public:
    string removeDuplicateLetters(string s) {
        vector<int> cnt(26, 0);
        vector<int> visited(26, 0);
        string ans=  "0"; 
        for (auto& c: s) cnt[c-'a']++;
        
        for (auto& c: s) {
            --cnt[c-'a'];
            if (visited[c-'a']) continue;
            while (ans.back() > c && cnt[ans.back()-'a']>0) {
                visited[ans.back()-'a']--;
                ans.pop_back();
            } 
            ans += c;
            visited[c-'a']++;
            
        }
        return ans.substr(1);
    }
};
```

