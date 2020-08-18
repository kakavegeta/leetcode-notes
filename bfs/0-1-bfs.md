

### [1368. Minimum Cost to Make at Least One Valid Path in a Grid](https://leetcode.com/problems/minimum-cost-to-make-at-least-one-valid-path-in-a-grid/)

Here is the 0-1 bfs solution. And you can see BFS-DFS solution in leetcode-notes/graph/dfs-bfs-comb.md

```c++

class Solution {
public:
    int minCost(vector<vector<int>>& grid) {
        vector<vector<int>> dirs = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        deque<vector<int>> dq;
        dq.push_front({0, 0, 0}); //r, c, cost
        int m = grid.size();
        int n = grid[0].size();
        vector<vector<int>> visited(m, vector<int>(n, 0));
         
        while (!dq.empty()) {
            auto cur = dq.front(); dq.pop_front();
            int r = cur[0], c = cur[1], cost = cur[2];
            if (r==m-1 && c==n-1) return cost; 
            if (visited[r][c]) continue;
            else visited[r][c] = 1;
            for (int i=0; i<4; ++i) {
                int nr = r+dirs[i][0];
                int nc = c+dirs[i][1];              
                if (nr >= m || nc >= n || nr < 0 || nc < 0 || visited[nr][nc]) continue;
                if (grid[r][c] == i+1) dq.push_front({nr, nc, cost});
                else dq.push_back({nr, nc, cost+1});
            }
        }

        return -1;
        
        
    }
};
```

