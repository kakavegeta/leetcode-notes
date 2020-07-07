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





