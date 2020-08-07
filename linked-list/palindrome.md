### Review

If not requiring no extra space, simply use a stack. Otherwise, find mid point of linked list, split, and reverse the second one. When find mid point, one thing needs to notice is length of linked list, not including last null pointer, whether even or odd matters the node begins to be reversed.



### [234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

#### idea

Notes: 

a. the length of linked list matters. if len<=1, simply return true; 

b. mid point finding scheme:

```c++
// if len is even, slow will be the second mid point
// if len is odd, slow is the mid point
ListNode* slow = head;
ListNode* fast = head;
while (fast && fast->next) {
	slow = slow->next;
    fast = fast->next->next;
}

// if len is even, slow will be the first mid point
// if len is odd, slow is the mid point
ListNode* slow = head;
ListNode* fast = head;
while (fast->next && fast->next->next) {
	slow = slow->next;
    fast = fast->next->next;
}
```

#### code

```c++
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if (!head || !head->next) return true;
        ListNode* slow = head;
        ListNode* fast = head;
        
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        if (fast) slow = slow->next;
        slow = reverse(slow);
        while (slow) {
            if (slow->val != head->val) return false;
            slow = slow->next;
            head = head->next;
        }
        return true;
    }
    
    ListNode* reverse(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* cur = head;
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





