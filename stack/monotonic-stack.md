### Review

Monotonic stack is a simple structure carrying not-so-simple operations however. Generally speaking, monotonic stack is a stack whose elements are in increasing/decreasing order. But how we build such a stack and where should we use this data structure? For the latter, a sketchy answer is that when we meet a problem that asks for next greater/smaller number, we can definitely employ this data structure. As for more detail, let's investigate several problems. But firstly throw one template for building monotonic stack from an array:

```c++
stack<int> build_monotonic_stack(vector<int> arr) {
    stack<int> stk;
    for (int a: arr) {
		// pop out all element smaller than current number in array
        // to make sure all elements in stack in decreasing order.
        while (!stk.empty() && stk.top() < a) {
            // generally some operations here
            stk.pop(); 
        }
        stk.push(a);
    }
    return stk;
}
```





### [503. Next Greater Number II](https://leetcode.com/problems/next-greater-element-ii/)

#### idea

There is a simpler version [496](https://leetcode.com/problems/next-greater-element-i/) which is the most basic usage of monotonic stack. Let us look one more interesting. Now we are in a circular array. In fact, however, there is no real circular array in memory. However, through operation `%` , we can simulate effect of circle in twice loop. 

#### code

```c++
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        stack<int> stk;
        int sz = nums.size(); 
        vector<int> ans(sz, -1);
        // use i%sz to implement loop array
        for (int i=0; i<2*sz; ++i) {
            while(!stk.empty() and nums[stk.top()] < nums[i%sz]) {
                ans[stk.top()] = nums[i%sz]; stk.pop();
            }
            stk.push(i%sz);
        }
        return ans;
    }
};
```

You see here we store index in stack rather than element of array. In 496 we can store element and use a hash table to record each element's next greater number, because in 496 elements in array are unique. But now elements are not unique, therefore, some correct answer might be overwritten wrongly. For example, [6,1,5,1,6], the first 1's next greater is 5 and second 1's is 6. If store elements with hash table, the the first 1's next greater would be overridden with 6. To prevent that, we need to store index. 

### [907. Sum of Subarray Minimums](https://leetcode.com/problems/sum-of-subarray-minimums/)

#### idea

This problem is more complicated. It can be also decomposed to next greater problem, but we need to consider two directions. For example, given an array [3, 1, 2, 4], for each number `num`, we need to find the number `left`  of left-forward sub-arrays ending with `num` , and the number `right` of right-forward sub-arrays starting with `num`.  Therefore, the number of all sub-arrays with `num` as minimum would be `left*right`, and the sum for this `num` is `left*right*num`. We need to sum up all those sub-array sum. 

#### code

```c++
class Solution {
public:
    int sumSubarrayMins(vector<int>& A) {
        int sz = A.size();
        stack<int> prev, next;
        vector<int> left(sz, -1), right(sz, -1);
        
        int mod = 1e9+7;
        // find right-forward next smaller
        for (int i=0; i<sz; ++i) {
            while (!next.empty() && A[next.top()] > A[i]) {
                int cur_idx = next.top(); next.pop();
                right[cur_idx] = i;
            }
            next.push(i);
        }
        // find right-forward next smaller
        for (int i=0; i<sz; ++i) {
            while (!prev.empty() && A[prev.top()] > A[i]) {
                prev.pop(); 
            }
            left[i] = prev.empty()? -1: prev.top();
            prev.push(i);
        }
        
        int ans = 0;       
        for (int i=0; i<sz; ++i) {
            // if left[i] == -1, then no left smaller for A[i], 
            // the number of left greaters should be i+1
            int l = left[i] == -1 ? -1 : left[i];
            // if right[i] == -1, then no right smaller for A[i],
            // the number of right greater should be sz-i
            int r = right[i] == -1 ? sz : right[i];
            ans = (ans + (i-l)*(r-i)*A[i]) % mod;
        }
        return ans;
    }
};
```

Or we can simply count rather than make operations on index

```c++
class Solution {
public:
    int sumSubarrayMins(vector<int> A) {
        int res = 0, n = A.size(), mod = 1e9 + 7;
        vector<int> left(n), right(n);
        stack<pair<int, int>> s1,s2;
        for (int i = 0; i < n; ++i) {
            int count = 1;
            while (!s1.empty() && s1.top().first > A[i]) {
                count += s1.top().second;
                s1.pop();
            }
            s1.push({A[i], count});
            left[i] = count;
        }
        for (int i = n - 1; i >= 0; --i) {
            int count = 1;
            while (!s2.empty() && s2.top().first >= A[i]) {
                count += s2.top().second;
                s2.pop();
            }
            s2.push({A[i], count});
            right[i] = count;
        }
        for (int i = 0; i < n; ++i)
            res = (res + A[i] * left[i] * right[i]) % mod;
        return res;
    }
};
```

Let's see more complicated problems

### [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

#### idea

It is almost same as 907. The only difference emerges in the calculation returned. In this problem, we should plus the left-forward length and right-forward length, rather than multiply.

#### code

```c++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<int> stk_next, stk_prev;
        
        int n = heights.size();
        
        vector<int> next_lesser(n, -1);
        vector<int> prev_lesser(n, -1);
        for (int i=0; i<n; ++i) {
            while (!stk_next.empty() && heights[stk_next.top()] > heights[i]) {
                int cur = stk_next.top(); stk_next.pop();
                next_lesser[cur] = i;
            }     
            stk_next.push(i);
        }
        
        for (int i=0; i<n; ++i) {
            while (!stk_prev.empty() && heights[stk_prev.top()] > heights[i]) {
                stk_prev.pop();
            }     
            prev_lesser[i] = stk_prev.empty()? -1: stk_prev.top();
            stk_prev.push(i);
        }
        
        int ans = 0;
        for (int i=0; i<n; ++i) {
            int nl_idx = next_lesser[i] == -1 ? n: next_lesser[i];
            int pl_idx = prev_lesser[i] == -1 ? -1: prev_lesser[i];
            // only differ here with 907
            ans = max(ans, heights[i]*(nl_idx-pl_idx-1)); 
        }
        return ans;
    }
};
```

But let's see a more beautiful solution due to [sipiprotoss5](https://leetcode.com/problems/largest-rectangle-in-histogram/discuss/28905/My-concise-C%2B%2B-solution-AC-90-ms).  Super Awesome! The basic idea of following solution is that for a number x, its previous greater number's index has already been stored in a increasing stack, therefore we can do all operations in one for loop. 

```c++
class Solution {
public:
    int largestRectangleArea(vector<int> &height) {
        stack<int> stk;
        height.push_back(0);
        int n = height.size();
        int ans = 0;
        
        for (int i=0; i<n; ++i) {
            while(!stk.empty() and height[stk.top()] >= height[i]) {
                int mid = stk.top(); stk.pop();
                int h = height[mid];
                int pre = stk.empty()? -1: stk.top();
                int next = i;
                ans = max(ans, h*(next-pre-1));
            }
            stk.push(i);
        }
        return ans;
    }
};
```



### [85. Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/)

#### idea





