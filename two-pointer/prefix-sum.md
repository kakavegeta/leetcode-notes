### Review



### [525. Contiguous Array](https://leetcode.com/problems/contiguous-array/)

#### idea

One approach is HashTable, the other is sliding window.  

#### code

```c++

//HashTable
class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        // notice, we should initially insert {0,-1}, since pos[cnt] = i
        unordered_map<int, int> pos{{0,-1}};
        int n = nums.size();
        int cnt = 0, ans = 0;
        for (int i=0; i<n; ++i) {
            if (nums[i] == 1) cnt++;
            else cnt--;
            if (pos.find(cnt) != pos.end()) ans = max(ans, i - pos[cnt]);
            else pos[cnt] = i;
        }
        return ans;
    }
};

// Use array as HashTable. One trick is, since cnt can be negative, the array should 
// initialized with size 2*n+1, here n is the original 0
class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        int n = nums.size();
        vector<int> pos(2*n+1, -2);
        // cnt can be negative, there n is the original point
        pos[n] = -1;
        int cnt = n, ans = 0; 
        for (int i=0; i<n; ++i) {[930. Binary Subarrays With Sum](https://leetcode.com/problems/binary-subarrays-with-sum/)52
            if (nums[i] == 1) cnt++;
            else cnt--;
            if (pos[cnt] != -2) ans = max(ans, i - pos[cnt]);
            else pos[cnt] = i;
        }      
        return ans;
    }
};
```



### [930. Binary Subarrays With Sum](https://leetcode.com/problems/binary-subarrays-with-sum/)

#### idea

This problem is almost same as 525 above. Also you can solve it by hash table or sliding window

```c++
// sliding window
// keep in mind the sequence of steps in for loop, only in such sequence problem can be solved
/*
So the template for sliding window:
1st: check whether sliding window conform to requirement
2nd: when jump out of while loop, window_sum must < or = S. We must first check
	< condition
3rd: we have to put chekcing = condition at last, because only in this way, we can increment ans.
4th: at last, since 0 does not impact window_sum, we have to iterate over 0s.
*/
class Solution {
public:
    int numSubarraysWithSum(vector<int>& A, int S) {
        int n = A.size();
        int lo = 0, hi = 0; 
        int window_sum = 0, ans = 0;
        for (;hi < n; hi++) {
            window_sum += A[hi];
            while (lo < hi && window_sum > S) window_sum -= A[lo++];
            if (window_sum < S) continue;
            if (window_sum == S) ans++;
            for (int j = lo; j<hi && A[j] == 0; j++) ans++;
        }
        return ans;
    }
};

// hash table
class Solution {
public:
    int numSubarraysWithSum(vector<int>& A, int S) {
        int n = A.size();
        vector<int> cnt(2*n+1, 0);
        cnt[n] = 1;
        int cursum = n, ans = 0; 
        for (int i=0; i<n; ++i) {
            cursum += A[i];
            if (cnt[cursum - S] != 0) ans += cnt[cursum - S];
            cnt[cursum]++;
        }
        return ans;
    }
};
```



