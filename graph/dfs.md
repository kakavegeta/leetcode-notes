### Review

DFS can be employed to find circles. The detail lies in devil: you need to handle different state.

### [802. Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)

#### idea

use DFS to find cycle, and any node accessible to this cycle will be labeled as UNSAFE.  Remember, in DFS recursion, __we should firstly check whether this node is visiting!__

#### code

```c++
class Solution {
public:  
    vector<int> state; // -1:unvisited 0:visiting,1:unsafe,2:safe
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        int n = graph.size();
        state = vector<int>(n, -1);
        vector<int> ans;
        for (int i=0; i<n; ++i) {
            if (dfs(i, graph) == 2) ans.push_back(i);
        }
        return ans;        
    }
    
    int dfs(int s, vector<vector<int>>& g) {
        // if s is visiting, we meet s again, s is unsafe.
        if (state[s] == 0) return state[s] = 1;
        if (state[s] != -1) return state[s];
        state[s] = 0;
        for (auto nb: g[s]) {
            // any node connecting to unsafe nodes will be unsafe. 
            if (dfs(nb, g) == 1) return state[s] = 1;
        }
        return state[s] = 2;
    }
};
```



### [207. Course Schedule](https://leetcode.com/problems/course-schedule/)

#### idea

Employ DFS to find circle. If no courses in circle, return true. 

#### code

```c++
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        int n = numCourses;
        state = vector<int>(n, 0);
        graph = vector<vector<int>>(n, vector<int>{});
        for (auto& cs: prerequisites) {
            graph[cs[0]].push_back(cs[1]); 
        }
        for (int i=0; i<n; ++i) {
            if (isCycle(i)) return false;
        } 
        return true;
    }  
private:
    vector<int> state; // 0: unvisited, 1:visiting, 2: visited
    vector<vector<int>> graph;
    bool isCycle(int s) {
        if (state[s] == 1) return true;
        if (state[s] == 2) return false;     
        state[s] = 1;
        for (int nb: graph[s]) {
            if (isCycle(nb)) return true;
        }
        state[s] = 2;
        return false;
    }    
};
```

