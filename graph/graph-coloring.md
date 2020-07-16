### Review

When the problem asks you do something which requires that no two adjacent vertex have same property, you can employ graph coloring method, which can be achieved either in DFS or BFS. 

### [886. Possible Bipartition](https://leetcode.com/problems/possible-bipartition/)

#### idea

No two adjacent vertex should be in same partition. If meet conflict, return false

#### code

```c++
// DFS
class Solution {
public:
    bool possibleBipartition(int N, vector<vector<int>>& edges) {
        vector<vector<int>> graph(N+1, vector<int>{});
        for (auto& edge: edges) {
            graph[edge[0]].push_back(edge[1]);
            graph[edge[1]].push_back(edge[0]);
        }
        
        // -1: unvisited, 0 vs 1
        vector<int> state(N+1, -1);
        for (int i=1; i<=N; ++i) {
            if (state[i] == -1 && !dfs(i, 0, graph, state)) return false;
        }
        return true;
        
    }
    
    bool dfs(int u, int color, vector<vector<int>>& graph, vector<int>&state) {
        state[u] = color;
        for (int v: graph[u]) {
            if (state[v] == color) return false;
            if (state[v] == -1 && dfs(v, 1-color, graph, state)==false) return false;
        }
        return true;
 	}
}
```

```C++
//BFS
class Solution {
public:
    bool possibleBipartition(int N, vector<vector<int>>& edges) {
        vector<vector<int>> graph(N+1, vector<int>{});
        for (auto& edge: edges) {
            graph[edge[0]].push_back(edge[1]);
            graph[edge[1]].push_back(edge[0]);
        }
        
        // -1: unvisited, 0 vs 1
        vector<int> state(N+1, -1);
        // have to iterate every node to avoid non-connected part. 
        for (int i=1; i<=N; ++i) {
            if (state[i] != -1) continue;
            queue<int> q;
            q.push(i);
            while (!q.empty()) {
                int cur = q.front(); q.pop();
                int col = state[cur];
                for (int nb: graph[cur]) {
                    if (state[nb] == -1) {
                        state[nb] = 1-col;
                        q.push(nb);
                    }
                    if (state[nb] == col) return false;
                }
            }
        } 
        return true;
    }
};
```

