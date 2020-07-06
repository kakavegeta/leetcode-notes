### Review

If you meet questions in which the order of node's values matters, in some probability, monotonic stack could be useful. Increasing/decreasing stack might be the most fitted approach to solve next greater/lesser questions.   More detail, see [mono-stack][../stack/mono-stack.md]



###  654. Maximum Binary Tree

#### idea

By finding out position of maximum node, it can be easily solved by recursive approach. However, after a close investigation, we can decompose it into a next greater problem. Given example [3,2,1,6,0,5], 3's next greater is 6, and thus 3's parent is 6, and 3 is 6's left child. {2,1} should be 3's right subtree. Generally speaking, we need to iterate nodes, and use a stack to keep a decreasing order. That is,  when incomes a node, "squeeze" the stack until a node whose value is greater than the incoming node. The bigger node, if exist, should still be in stack and be the parent of incoming node, or if the incoming node is the biggest node, the stack should be empty, and the last node popped out should be left child of incoming number. 

#### code

```c++
class Solution {
public:
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        vector<TreeNode*> stk;
        for (int i=0; i<nums.size(); ++i) {
            TreeNode* node = new TreeNode(nums[i]);
            while (!stk.empty() && stk.back()->val < nums[i]) {
                node->left = stk.back();
                stk.pop_back();
            }
            if (!stk.empty()) stk.back()->right = node;
            stk.push_back(node);
        }
        return stk[0];
    }
```



### 1330. Minimum Cost Tree From Leaf Value

#### idea

Of course we can solve this problem by dynamic programming. But another fantastic approach is greedy algorithm. Overall, we need to sum up a series of product of a pair of node values. To minimize this sum, we should try our best to minimize every product. We run a loop until no leaf node is available. In each loop, we find the minimum node, making product with its smaller neighbor, and then pop out this minimum node since it is no longer useful, and then add this product into final sum. 

But it is costly to find minimum node in every loop. Here comes stack! Remember we can use stack to build an monotonic array and can thus solve next greater problem. But in this problem, we need to find next greater in two neighbors. 

```c++
class Solution {
public:
    int mctFromLeafValues(vector<int>& arr) {
        stack<int> stk;
        int ans=0;
        
        for (int a: arr) {
            while (!stk.empty() && stk.top() < a) {
                int mid = stk.top(); stk.pop();
                // next greater in left and right neighbors. 
                if (stk.empty()) ans += mid*a;
                else ans += min(stk.top(), a) * mid; 
            }
            stk.push(a);
        }
        
        while (stk.size() > 1) {
            int n = stk.top(); stk.pop();
            ans += n * stk.top();
        }
        return ans;
    }
};
```

 



