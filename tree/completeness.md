### Review

You need to completely understand the property of complete binary tree. 



### [958. Check Completeness of a Binary Tree](https://leetcode.com/problems/check-completeness-of-a-binary-tree/)

#### BFS

For a full complete binary tree, every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. Therefore, we traverse the tree level by level, once we meet a null pointer before a node, return false. 

```c++
class Solution {
public:
    bool isCompleteTree(TreeNode* root) {
        if (!root) return true;
        queue<TreeNode*> q;
        q.push(root);
        bool missing = false;
        while (!q.empty()) {
            int sz = q.size();
            while (sz--) {
                TreeNode* cur = q.front(); q.pop();
                if (cur) {
                    if (missing) return false;
                    q.push(cur->left);
                    q.push(cur->right);
                } else {
                    missing = true;
                }
            }
        }
        return true;
    }
```

#### DFS

Use the depth property of complete binary tree. Except the last level with depth `d`,  any node in `d-1` level must be full, and in depth `d`, nodes should be arranged as far left as possible.  Given this property, we set a flag `last_level_filled`, if true, means that we meet a null pointer at last level, and from then, we should not meet any node, and any null pointer's depth should be `target_height-1`. 

credit to [@votrubac](https://leetcode.com/problems/check-completeness-of-a-binary-tree/discuss/205699/C%2B%2BJava-track-leftmost-height), excellent solution!

```c++
class Solution {
public:
    int target_height = 0, last_level_filled = false;
    bool isCompleteTree(TreeNode* r, int h = 0) {
    if (r == nullptr) {
        if (target_height == 0) {
            target_height = h;
        } else if (h == target_height - 1) {
            last_level_filled = true;
        }
        return h == target_height - last_level_filled;
      }
      return isCompleteTree(r->left, h + 1) && isCompleteTree(r->right, h + 1);
    }
};
```



### [919. Complete Binary Tree Inserter](https://leetcode.com/problems/complete-binary-tree-inserter/)

Notice that many binary tree design problem can be solve with queue, deque, or vector

