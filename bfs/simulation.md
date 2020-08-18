### Review

You might meet a kind of problems that describe a scenario which is very long and twisted... But after you put lens on it you can identity the core: BFS. But not simple BFS. Instead, You might need to carefully simulate the process. 



### [1298. Maximum Candies You Can Get from Boxes](https://leetcode.com/problems/maximum-candies-you-can-get-from-boxes/)

A hard one, but only in description. Once you smell out BFS behind this story, every thing is clear. One key: only put a box into queue when: a) you have a key of the box, and b) you have already got that box. Therefore, we need to use two array `usableBoxes[]` and `canOpen[]` to track each box's state.

```c++
class Solution {
public:
    int maxCandies(vector<int>& status, vector<int>& candies, vector<vector<int>>& keys, vector<vector<int>>& containedBoxes, vector<int>& initialBoxes) {
        int n = status.size();
        vector<int> usableBoxes(n, 0);
        vector<int> canOpen(status);
        queue<int> q;
        for (auto& init: initialBoxes) {
            usableBoxes[init] = 1; 
            if (status[init]) q.push(init);
        }
        int ans = 0;
        while (!q.empty()) {
            int cur = q.front(); q.pop();
            ans += candies[cur];
            
            for (auto& box: containedBoxes[cur]) {
                usableBoxes[box] = true; 
                // until the box is usable and has key we put it in queue 
                if (canOpen[box]) q.push(box);
            }
            
            for (auto& box: keys[cur]) {
                // avoid duplicate
                if (!canOpen[box] && usableBoxes[box]) q.push(box);
                canOpen[box] = 1;
            }
        }
        return ans;     
    }
};
```

