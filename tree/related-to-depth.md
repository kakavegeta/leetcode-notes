#### Review

Firstly let's look at how to get a tree's depth.

```c++
// simple recursion
int depth(Node *root) {
    if (!roor) return 0;
    return max(depth(root->left), depth(root->right))+1;
}
```

As a node's state, in scenario of questions, depends on its depth, which matters the way we come up solutions, we need to get depth of a node to do further operations.



#### 1302. Deepest Leaves Sum

##### idea

To sum up only deepest leaves, a straight way is that find maximum depth at first, storing <node, depth> information in a hash table, then sum up all nodes' values with maximum depth. But in this way we need to traverse the tree twice.  In fact we could perform updating maximum depth and summing up responding values at one traverse.

##### code

```c++
class Solution {
public:
    int max_depth;
    int sum;
    
    int deepestLeavesSum(TreeNode* root) {
        max_depth = 0;
        sum = 0;
        dfs(root, 0);
        return sum;
    }
    // solve in a recursive manner which illustrates how depth
    // will be used. But just traverse level might be more simple. 
    void dfs(TreeNode* root, int depth) {
        if (!root) return;
        // if meet larger depth, update max depth, and record the node's value which
        // must be a candidate of deepest leaves. 
        if (depth > max_depth) {
            max_depth = depth; 
            sum = root->val;
        } else if (depth == max_depth) {
            sum += root->val;
        }
        if (root->left) dfs(root->left, depth+1);
        if (root->right) dfs(root->right, depth+1);
    }
```

 