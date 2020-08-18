### Review

Generally, two pointers problem need to employ two pointers on single array or string. However, sometimes we can also use two pointers iterate through two arrays or strings, one for each. 



### [844. Backspace String Compare](https://leetcode.com/problems/backspace-string-compare/) 

#### idea

Stack is suitable there. But follow up asks O(1) space: we should not use extra space. So you see, two pointers trick can not only reduce time complexity but also space complexity. The tricky spot is here: since # implies backspace operation, if we iterate from front to back, when we meet a non-# character, we cannot know whether such character should be deleted, so we need extra space to record the state. But wait, what if we iterate from back to front? In this way, when we meet non-# character, we can easily judge whether it should be deleted by a variable which count the number of # remain. 

#### code

```c++
class Solution {
public:
    bool backspaceCompare(string s, string t) {
        int i = s.size()-1, j = t.size()-1;
        int back1 = 0, back2 = 0; 
        while (1) {
            // move i to front until # is used up
            while (i >= 0 && (s[i] == '#' || back1 > 0)) s[i--] == '#'? ++back1: --back1;
            // move j to front until # is used up
            while (j >= 0 && (t[j] == '#' || back2 > 0)) t[j--] == '#'? ++back2: --back2;
            // if i and j still >=0, check s[i]==s[j], if true, mean until now s and t are  equal, so --i and --j
            if (i >=0 && j >= 0 && s[i] == t[j]) {--i; --j;}
            else break; 
        }
        // the reason why we jump out of while(1) loop is (i<0 && j<0) or (s[i]!=t[j]).
        // if s[i]!=t[j], just return false. Otherwise, if i<0 and j<0, means we finish 
        // iterations on both and no unmatch, so return true. 
        return i==-1 && j==-1;
    }
};
```

