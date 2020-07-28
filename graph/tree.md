### Review

Undirected graph can degrade to tree. 



### [1519. Number of Nodes in the Sub-Tree With the Same Label](https://leetcode.com/problems/number-of-nodes-in-the-sub-tree-with-the-same-label/)

#### idea

Initially I miss a point that edge is given without order. You need to find which is parent and which is child by self. One way is to restrict the relationship in graph traversal, another is to build such a tree initially(this method will depend the order of edges given, ignore).

#### code

```c++
class Solution {
public:
    
    vector<int> ans;
    vector<int> countSubTrees(int n, vector<vector<int>>& edges, string labels) {
        ans = vector<int>(n, 0);
        vector<vector<int>> graph(n, vector<int>{});
        for (auto& edge: edges) {
            graph[edge[0]].push_back(edge[1]);
            graph[edge[1]].push_back(edge[0]);
        }
        dfs(graph, 0, -1, labels);
        return ans;
       
    }
    // restrict the parent-child relation
    vector<int> dfs(vector<vector<int>>& graph, int root, int parent, string& labels) {
        vector<int> cnt(26, 0);
        char label = labels[root];
        for (int child: graph[root]) {
            if (child != parent) {
                vector<int> _cnt = dfs(graph, child, root, labels);
                for (int i=0; i<26; ++i) {
                    cnt[i] += _cnt[i];
                }
            }
        }
        ans[root] = ++cnt[label-'a'];
        return cnt;
    }
};
```

Just get another simpler and cooler code from [Lin](https://www.youtube.com/watch?v=izGTqs-4y-I&t=194s) . Same idea but more clean way. 

```c++
class Solution {
public:
    vector<vector<int>> adj;
    vector<int> cnt;
    vector<int> ans;
    string lbs;
    void dfs(int u, int p) {
        // previous level's count, must be substracted at end.
        int c = cnt[lbs[u]-'a']++;
        for (int v: adj[u]) 
            if (v != p) dfs(v, u);
        ans[u] = cnt[lbs[u]-'a']-c; 
    }
    vector<int> countSubTrees(int n, vector<vector<int>>& edges, string labels) {
        adj = vector<vector<int>>(n);
        lbs = labels;
        cnt = vector<int>(26, 0);
        ans = vector<int>(n, 0);
        
        for (auto& e: edges) {
            adj[e[0]].push_back(e[1]);
            adj[e[1]].push_back(e[0]);
        }
        
        dfs(0, -1);
        return ans;
    }
```

