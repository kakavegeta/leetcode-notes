### Review

Some problems about islands/water/flood can be more efficiently solved by BFS than DFS. Although both in time complexity O(V+E), BFS might reach results more quickly. 

### [1162. As Far from Land as Possible](https://leetcode.com/problems/as-far-from-land-as-possible/) 

#### idea

Add all islands into queue as source, and perform BFS for water cell. The last reached water cell will be the desired one~. Remember, this is a kind of multi-source BFS, but as (suppose, not actually) you put one meta-source island on top of all  islands at beginning, the multi-source BFS is no different with normal single-source BFS.

#### code

```c++
class Solution {
public:
    int maxDistance(vector<vector<int>>& grid) {
        queue<pair<int, int>> q;
        int m = grid.size();
        int n = grid[0].size();
        for (int i=0; i<m; ++i) {
           for (int j=0; j<n; ++j) {
               if (grid[i][j] == 1) q.push({i, j});
           }
        }
        if (q.empty() || q.size()==m*n) return -1;
        
        vector<pair<int, int>> dir = {{0,1},{0,-1},{1,0},{-1,0}};
        int x, y;
        int ans = 0, dist = 0;
        while (!q.empty()) {
            x = q.front().first;
            y = q.front().second;
            q.pop();
            for (int i=0; i<4; ++i) {
                int newx = x + dir[i].first;
                int newy = y + dir[i].second;
                // if out of boundary or visited, next~
                if (newx < 0 || newx >= m || newy < 0 || newy >=n || grid[newx][newy] != 0) continue;
                // simply add one, bur remember, since for islande (i,j) grid[i][j]==1, 
                // you need to substract one from fianl water (x,y) grid[x][y] to get 
                // correct answer.
                grid[newx][newy] = grid[x][y] + 1;
                q.push({newx, newy});
            }
        }
        return grid[x][y] - 1;
    }
};
```



