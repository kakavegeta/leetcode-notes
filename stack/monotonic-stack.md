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





### 503. Next Greater Number II

[503]https://leetcode.com/problems/next-greater-element-ii/

#### idea

There is a simple version 