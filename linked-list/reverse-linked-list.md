### Review

There are two way to reverse linked list: iteration and recursion. 



### [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

#### code

```c++
// recursion
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (!head) return head;
        ListNode* newhead = reverseList(head);
        head->next->next = head;
        head->next = nullptr;
        return newhead; 
    }
};

// iteration
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* cur = head;
        if (!cur) return head;      
        ListNode* prev = nullptr;
        while (cur) {
            ListNode* tmp = cur->next;
            cur->next = prev;
            prev = cur;
            cur = tmp; 
        }       
        return prev;
    }
};
```

