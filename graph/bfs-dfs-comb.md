### [1368. Minimum Cost to Make at Least One Valid Path in a Grid](https://leetcode.com/problems/minimum-cost-to-make-at-least-one-valid-path-in-a-grid/)

A great problem! One key insight to finish it is to treat the grid as a graph: each sign is a node, and the weight is 0 if same, 1 if different with neighbor. 

Since we need to figure out the minimum cost, we can use a dp matrix to store states. Dijkstra is good, but since the weight is either 0 or 1, 0-1 BFS might be better. Here you can see that the core in Dijkstra is greedy+BFS.

To see Dijkstra or 0-1 BFS  method, go to leetcode-notes/bfs/0-1-bfs.md

Here I show the BFS+DFS method. The idea is simple:

1) use DFS to find a reachable path from (0,0), marked all nodes in this path as visited, and push all nodes to queue. In this path, the cost of every node is 0 since we change no signs

2) do BFS until queue is empty:

​		for each node poped, DFS all its 3 other directions;

I just realized that this is multi-source BFS in 1). That is, put all reachable nodes from (0,0) without changing sign at first. 

```c++
class Solution {
public:
    vector<vector<int>> graph;
    vector<vector<int>> dp;
    queue<pair<int, int>> q;
    vector<vector<int>> dirs;
    int m, n, cost; 
    int minCost(vector<vector<int>>& grid) {
        m = grid.size();
        n = grid[0].size();
        cost = 0;
        dirs = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        graph = grid;
        dp = vector<vector<int>>(m, vector<int>(n, -1));
        dfs(0, 0); // initialize the queue firstly
        while (!q.empty()) {
            cost++; 
            // here we do bfs level by level because everytime we enter next level, cost+1
            int sz = q.size();
            while (sz--) {
                auto [r, c] = q.front(); q.pop();
                for (int i=0; i<4; ++i) {
                    dfs(r+dirs[i][0], c+dirs[i][1]);
                }
            }
        }
        return dp[m-1][n-1];
    }
    
    void dfs(int r, int c) {
        if (r<0 || c<0 || r>=m || c>=n || dp[r][c]>=0) return;
        dp[r][c] = cost;
        q.push({r, c});
        int nextDir = graph[r][c]-1;
        dfs(r+dirs[nextDir][0], c+dirs[nextDir][1]); 
    }
};
```



