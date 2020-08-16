### [1368. Minimum Cost to Make at Least One Valid Path in a Grid](https://leetcode.com/problems/minimum-cost-to-make-at-least-one-valid-path-in-a-grid/)

A great problem! One key insight to finish it is to treat the grid as a graph: each sign is a node, and the weight is 0 if same, 1 if different with neighbor. 

Since we need to figure out the minimum cost, we can use a dp matrix to store states. Dijkstra is good, but simple BFS is also ok. Here you can see that the core in Dijkstra is greedy+BFS.



