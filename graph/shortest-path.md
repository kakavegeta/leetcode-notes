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

Note: if you can tell the difference between Dijkstra and Bellman-Ford, you might get the essence of the two algorithms. Generally speaking, Dijkstra update each 

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





