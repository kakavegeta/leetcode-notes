### [138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

Hash table is a common tool to solve copy/clone problems. Simply mapping original node to cloned node works well. Notice that you have to iterate twice: the first for scan all nodes and finish establishing hash table, and the second to assign random pointer. You have to firstly scan all nodes to establish hash table, otherwise your random pointer assignment might loss some nodes. 

```c++
// Hash Table

// Iteration
class Solution {
public:
    unordered_map<Node*, Node*> ht;
    Node* copyRandomList(Node* head) {
        if (!head) return nullptr;
        Node* copy = new Node(head->val); 
        ht[head] = copy;
        Node* cur = head->next;
        while (cur) {
            Node* _cur = new Node(cur->val);
            ht[cur] = _cur;
            copy->next = _cur;
            cur = cur->next;
            copy = copy->next;
        }
        
        cur = head;
        copy = ht[head];
        while (cur) {
            copy->random = ht[cur->random];
            cur = cur->next;
            copy = copy->next;
        }
        return ht[head];
    }
};

// Recursion
class Solution {
public:
    unordered_map<Node*, Node*> ht;
    
    Node* copyRandomList(Node* head) {
        Node* newhead = clone(head);
        cloneRandom(head, newhead);
        return newhead;
    }
    
    Node* clone(Node* node) {
        if (node==nullptr) return nullptr;
        Node* copy = new Node(node->val);
        ht[node] = copy;
        copy->next = clone(node->next);
        return copy;
    }
    
    void cloneRandom(Node* node, Node* copy) {
        if (node==nullptr) return;
        copy->random = ht[node->random];
        cloneRandom(node->next, copy->next);
    }   
};
```



```c++
// do not use hash table
class Solution {
public:
    
    Node* copyRandomList(Node* head) {
        if (!head) return nullptr;
        
        Node* cur = head;
        while (cur) {
            Node* copy = new Node(cur->val);    
            copy->next = cur->next;
            cur->next = copy;
            cur = copy->next;
        }
        
        cur = head;
        while (cur) {
            Node* copy = cur->next;
            if(cur->random) copy->random = cur->random->next;
            cur = cur->next->next;
        } 
        
        cur = head;
        Node* newhead = cur->next;
        while (cur) {
            Node* copy = cur->next;
            cur->next = copy->next;
            cur = cur->next;
            if (cur) copy->next = cur->next;
        }
        return newhead;
 
    }
};
```

