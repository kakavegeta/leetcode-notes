### Review

The two pointer application can be very tricky. One example is finding mid point of a linked list. But let's see another tricky problem



### [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

#### idea

One approach is cutting length. Another fabulous one is employing the thought we used in cycle detection.  In this scenario, make the two pointers walk same length by alternating head when reaching end. Anyway,  awesome! 

#### code

```c++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if (!headA || !headB) return nullptr;
        
        ListNode* a = headA;
        ListNode* b = headB;
        
        while(a!=b) {
            a = a==nullptr?headB: a->next;
            b = b==nullptr?headA: b->next;
        }   
        return a;
    }
};
```



