### Review

Given two arrays with size m and n, sometimes you do not need to use a nested for loop to iterate all possible states, with complexity O(m*n). Instead, you can simply use two pointers with one pass iteration.



### [1537. Get the Maximum Score](https://leetcode.com/problems/get-the-maximum-score/)

#### idea

Dynamic programming. Given the two arrays A and B:

```pseudocode
dpa, dpb with size A.size()+1, B.size()+1
dpa[i]: maximum score until A[i-1]
dpb[j]: maximum score until B[j-1]

state 
```

