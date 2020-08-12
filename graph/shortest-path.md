### Review

For shortest path problem, detailed description see [shortest-path-problem](https://en.wikipedia.org/wiki/Shortest_path_problem).  Weighted/unweighted, directed/undirected, negative/nonnegative..., for different type of graphs, varying algorithms would be applied. But edge relaxation is the core part in no matter what algorithms. 

```pseudocode
d[v]: current shortest path from starting point
w(u, v): weight/distance between u-v
relax(u, v, w)
if d[v] > d[u] + w(u, v):
	d[v] = d[u] + w(u, v)
	parent[v] = u
```

There are some videos I think kind of illustrative:

[Dijkstra](https://www.youtube.com/watch?v=GazC3A4OQTE)

[Bellman-Ford](https://www.youtube.com/watch?v=obWXjtg0L64)



### [1514. Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/) 

#### idea

Simply transform it as shortest path problem which can be solved by BFS in way of Bellman-Ford algorithm. One difference is that in current scenario we do update probability if finding greater one instead of distance. 

#### code

```c++
class Solution {
public: 
    vector<vector<pair<int, double>>> g;
    double maxProbability(int n, vector<vector<int>>& edges, vector<double>& succProb, int start, int end) {
        g = vector<vector<pair<int, double>>>(n, vector<pair<int, double>>{});
        
        for (int i=0; i<edges.size(); ++i) {
            g[edges[i][0]].push_back({edges[i][1], succProb[i]});
            g[edges[i][1]].push_back({edges[i][0], succProb[i]});
        }
        
        queue<int> q;
        q.push(start);
        vector<double> probs(n, 0.0);
        probs[start] = 1.0; // important setting for start point.
        
        while (!q.empty()) {
            int cur = q.front();
            double curp = probs[cur];
            q.pop();
            for (auto it = g[cur].begin(); it != g[cur].end(); ++it) {
                // only update if we get greater probability.
                if (curp*it->second > probs[it->first]) {
                    probs[it->first] = curp*it->second;
                    q.push(it->first);
                }
            }
        }
        return probs[end];
    }     
};
```




### [743. Network Delay Time](https://leetcode.com/problems/network-delay-time/)

#### idea

Simply apply bellman-ford algorithm to find time cost from K to each vertex. Finally the max time cost will be the answer. 

#### code 

```c++
class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int N, int K) {
        vector<int> dist(N+1, INT_MAX);
        dist[K] = 0;
        for (int i=0; i<N; ++i) {
            for (auto& time: times) {
                int u=time[0], v=time[1], t=time[2];
                // relaxation
                if (dist[u]!=INT_MAX and dist[v] > dist[u] + t) dist[v] = dist[u]+t;
            }
        }  
        int ans = 0;
        for (int i=1; i<=N; ++i) {
            ans = max(ans, dist[i]);
        }
        return ans == INT_MAX? -1: ans;
    }
};
```



### [787. Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

#### Dijkstra

one excellent explanation why we do not use relaxation:

Some explanation.
The key difference with the classic Dijkstra algo is, we don't maintain the global optimal distance to each node, i.e. ignore below optimization:
`alt ← dist[u] + length(u, v)`
`if alt < dist[v]:`
Because there could be routes which their length is shorter but pass more stops, and those routes don't necessarily constitute the best route in the end. To deal with this, rather than maintain the optimal routes with 0..K stops for each node, the solution simply put all possible routes into the priority queue, so that all of them has a chance to be processed. IMO, this is the most brilliant part.
And the solution simply returns the first qualified route, it's easy to prove this must be the best route.



```c++
typedef tuple<int, int, int> ti;
class Solution {
public:
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K) {
        vector<int> dist(n, INT_MAX);
        
        vector<vector<pair<int, int>>> graph(n);
        for (auto &flight: flights) {
            graph[flight[0]].push_back({flight[1], flight[2]});
        }
        
        priority_queue<ti, vector<ti>, greater<ti>> pq;
        
        pq.push({0, src, 0});
        dist[src] = 0;
        
        while (!pq.empty()) {
            auto [du, u, depth] = pq.top(); pq.pop();
            
            if (u == dst) return du;
            if (depth == K+1) continue;
            for (auto& p: graph[u]) {
                auto [v, w] = p;
                // there, we do no relaxation. 
                pq.push({du+w, v, depth+1});
            }
        }
        return -1;
    }
};
```

#### BFS

Similar to Dijkstra, but use queue instead of priority queue

```c++
class Solution {
public:
    vector<vector<pair<int, int>>> graph;
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K) {
        graph = vector<vector<pair<int, int>>>(n);
        for (auto & flight: flights) {
            graph[flight[0]].push_back({flight[1], flight[2]});
        }
        int ans = INT_MAX;
        
        queue<pair<int, int>> q;
        q.push({src, 0});
        
        while (!q.empty() && K+1>=0) {
            int sz = q.size();
            while (sz--) {
                auto [u, du] = q.front(); q.pop();
                if (u == dst) ans = min(ans, du);
                for (auto [v, w]: graph[u]) {
                    if (du + w > ans) continue; // prunning, important!
                    q.push({v, du+w});
                }
            }
            K--;
        }
        return ans == INT_MAX? -1: ans;
    }
};
```

#### DFS

```c++
typedef pair<int, int> pi;
class Solution {
public:
    vector<vector<pi>> graph;
    vector<int> visited;
    int ans;
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K) {
        graph = vector<vector<pi>>(n);
        ans = INT_MAX;
        visited = vector<int>(n, 0); 
        visited[src] = 1;
        for (auto& flight: flights) {
            graph[flight[0]].push_back({flight[1], flight[2]});
        }
        
        dfs(src, dst, 0, K+1);
        return ans == INT_MAX? -1: ans;
    }
    
    void dfs(int src, int dst, int dsrc, int stops) {
        if (src == dst) {ans = dsrc; return;}
        if (stops == 0) return;
        for (auto& [v, ww]: graph[src]) {
            if (visited[v] == 1 || dsrc + ww > ans) continue; // prunning!
            visited[v] = 1;
            dfs(v, dst, dsrc+ww, stops-1);
            visited[v] = 0;
        }
        
    }  
};
```

