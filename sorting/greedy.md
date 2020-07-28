### Review

Some sorting problem need to be solved in greedy thought. In fact, a lot of greedy questions, often involving array, need to sort array at first. After all, greedy questions often ask you to find max/min solution, and if we want to make every current state optimal, sorting the array first is often necessary. Inversely, some sorting questions are compound with greedy thought.



### [1054. Distant Barcodes](https://leetcode.com/problems/distant-barcodes/submissions/)

#### idea

It ask you to rearrange an array so that no adjacent elements are equal. Greedy comes~ we need to arrange most frequent element first, then second frequent one, then third... But wait, as you inspect this process again and draw a picture, you will easily find that after we arrange the most frequent element, we just need to fill out space left with elements remained. That is, we do not have to sort elements by frequency, which will greatly reduce time complexity from O(nlogn) to O(n).

#### code

```c++
class Solution {
public:
    vector<int> rearrangeBarcodes(vector<int>& bs) {
        vector<int> cnt(10001, 0);
        // record maximum frequency and corresponding element
        int mx_cnt = 0, mx_b = 0;
        for (int b: bs) {
            mx_cnt = max(mx_cnt, ++cnt[b]);
            mx_b = mx_cnt==cnt[b] ? b: mx_b;
        }
        
        int n = bs.size();
        vector<int> ans(n);
        // initially from index 0
        int pos = 0;
        for (int i=0; i<10001; ++i) {
            int num = i==0 ? mx_b: i;
            while (cnt[num]-- > 0) {
                ans[pos] = num;
                // if reach end, restart from index 1.
                pos = pos + 2 >= n ? 1: pos+2;
            }
        }
        return ans;
    }
};
```

