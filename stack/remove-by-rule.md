### Review

Some problems ask you to remove elements in an array by defined rules. If the rule is related to new element encountered and previous one, it possible to be solved with stack structure (generally speaking it can also be solved by two pointer approach). 



### [1209. Remove All Adjacent Duplicates in String II](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string-ii/)

#### idea

Of course it can be solved by two-pointer approach. But stack can be employed here with almost same though. Save pair <element, count> in to stack, and If we meet duplicates, just increase count of duplicate, and if that the count of duplicate equals to k, we pop out that element. 

#### code

```c++
class Solution {
public:
    string removeDuplicates(string s, int k) {
        vector<pair<char, int>> cnt;
        cnt.push_back({'#', 0});
        for (char& c: s) {
            // if meet duplicates, increase count; if new one, push {c, 1}
            if (cnt.back().first == c) cnt.back().second++;
            else cnt.push_back({c, 1});
            // if cnt == k, pop out the element
            if (cnt.back().second == k) cnt.pop_back();
        }
        
        // build answer
        string ans = "";
        for (int i=1; i<cnt.size(); ++i) {
            ans.append(cnt[i].second, cnt[i].first);
        }
        return ans;

    }
};
```



### [735. Asteroid Collision](https://leetcode.com/problems/asteroid-collision/)

#### idea

Simply follow the requirement. But the condition might be tricky to set. We need to figure out that in what condition we should do collision, that is, only if new asteroid move left, and last asteroid move right. Otherwise, we need to push new asteroid into stack. 

#### code

```c++
class Solution {
public:
    vector<int> asteroidCollision(vector<int>& as) {
        vector<int> stk;
        int sz = as.size(); 
        for (int i=0; i<sz; ++i) {
            if (as[i] > 0 || stk.empty() || stk.back() < 0) stk.push_back(as[i]);
            else {
                if (stk.back() + as[i] <= 0) {
                    if (stk.back() + as[i] < 0) --i;
                    stk.pop_back();
                }
            }
        }
        return stk;
    }
};
```



