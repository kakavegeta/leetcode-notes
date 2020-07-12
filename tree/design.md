### Review

Some problems ask you design data structure using properties of tree such as BST.



### [173. Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

#### idea

The first comes to my mind is to use inorder traversal to get a increasing array, and record the position. But next() will cost O(n) memory while O(1) time. Then I think about stack, which cost O(h) memory and O(h) time. Almost same with inorder traversal with stack.  

```c++
class BSTIterator {
public:
    TreeNode* cur;
    stack<TreeNode*> stk;
    
    BSTIterator(TreeNode* root) {
        push(root);
        
    }
    
    int next() {
        TreeNode* cur = stk.top(); stk.pop();
        push(cur->right);
        return cur->val;
    }
   
    bool hasNext() {
        return !stk.empty();
    }
    
    void push(TreeNode* cur) {
        while (cur) {
            stk.push(cur);
            cur = cur->left;
        }
    }
};
```

