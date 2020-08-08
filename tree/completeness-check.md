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

