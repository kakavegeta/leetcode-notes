### Review

Tree traversal can be easily implemented by recursion. But it is more interesting in iterative way. For more details, please see [tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)



### [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

#### code

```c++
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode*> stk;
        vector<int> ans;
        
        TreeNode* cur = root;
        
        while (!stk.empty() || cur) {
            if (cur) {
                stk.push(cur);
                cur = cur->left;
            } else {
                cur = stk.top(); stk.pop();
                ans.push_back(cur->val);
                cur = cur->right;
            }
        }
        return ans;
    }
};
```

### [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

#### code

```c++
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if (!root) return {};
        stack<TreeNode*> stk;
        vector<int> ans;
        TreeNode* cur = root;
        stk.push(cur);
        
        while (!stk.empty()) {
            cur = stk.top(); stk.pop();
            ans.push_back(cur->val);
            if (cur->right) stk.push(cur->right);
            if (cur->left) stk.push(cur->left);
        }
        return ans;
    }
};
```

### [145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

#### idea

Postorder in iterative approach is a bit more complicated. The trick is that you need to keep track the node you last visited. 

```c++
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode*> stk;
        TreeNode* last = nullptr;
        vector<int> ans; 
        while (!stk.empty() || root) {
            if (root) {
                stk.push(root);
                root = root->left;
            } else {
                TreeNode* node = stk.top();
                if (node->right and last != node->right) {
                    root = node->right;
                } else {
                    ans.push_back(node->val);
                    last = stk.top(); stk.pop();
                }
            }
        }
        return ans;       
    }
};
```

