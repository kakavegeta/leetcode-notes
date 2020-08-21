### Review

Usually, for linked list traversal, iteration is kind of similar to recursion, and the amount of code is almost equal to some extant, unlike graph or tree. After all, linked list only has one path.

___For linked list traversal, notice that, only when cur!=nullptr, we can make operation cur->next. Therefore, keep alerted when we traverse to end!___

### [430. Flatten a Multilevel Doubly Linked List](https://leetcode.com/problems/flatten-a-multilevel-doubly-linked-list/)

```c++
// iteration
// flatten from top level to bottom level
class Solution {
public:
    Node* flatten(Node* head) {
        if (head==nullptr) return nullptr;
        
        Node* cur = head;
        while (cur) {
            if (cur->child) {
                Node* next = cur->next;
                Node* child = cur->child;
                cur->next = child;
                child->prev = cur;
                cur->child = nullptr;
                while (child->next) child = child->next;
                child->next = next;
                // often forget to check... Keep in mind!
                if (next) next->prev = child;
            } 
            cur = cur->next;
        }
        return head;
    }
};

// recursion
// flatten from bottom level to top level
class Solution {
public:
    Node* flatten(Node* head) {
        if (head==nullptr) return nullptr;
        
        Node* cur = head;
        while (cur) {
            if (cur->child) {
                Node* next = cur->next;
                // only one difference here...
                Node* child = flatten(cur->child);
                cur->next = child;
                child->prev = cur;
                cur->child = nullptr;
                while (child->next) child = child->next;
                child->next = next;
                if (next) next->prev = child;
            } 
            cur = cur->next;
        }
        return head;
    }
    
};
```



### [114. Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)

Almost same with 430

```c++
// recursion
class Solution {
public:
    void flatten(TreeNode* root) {
        if (!root) return;
        if (root->left) flatten(root->left);
        if (root->right) flatten(root->right);
        TreeNode* fright = root->right;
        root->right = root->left;
        root->left = nullptr;
        while (root->right) root = root->right;
        root->right = fright;
 
    }
};

// iteration
class Solution {
public:
    void flatten(TreeNode* root) {
        if (!root) return;
        while (root) {
            if (root->left) {
                TreeNode* right = root->right;
                root->right = root->left;
                root->left = nullptr;
                TreeNode* p = root;
                while (p->right) p = p->right;
                p->right = right;
            }
            root = root->right;
        }
    }
};
```

