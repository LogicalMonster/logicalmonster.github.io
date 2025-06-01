---
title: 算法训练营Day3
date: 2025-06-01 21:03:50
tags:
---
# 203. 移除链表元素

[203. 移除链表元素 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-linked-list-elements/description/)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode dummyNode = ListNode(0);
        dummyNode.next = head;
        ListNode* prev = &dummyNode;
        ListNode* curr = prev->next;
        while (curr != nullptr) {
            if (curr->val == val) {
                prev->next = curr->next;
            } else {
                prev = curr;
            }
            curr = curr->next;
        }
        return dummyNode.next;
    }
};
```

时间复杂度：O(n)
空间复杂度：O(1)

# 707. 设计链表

[707. 设计链表 - 力扣（LeetCode）](https://leetcode.cn/problems/design-linked-list/description/)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class MyLinkedList {
private:
    ListNode* head;
    size_t size;

public:
    MyLinkedList() : head{nullptr}, size{} {}

    int get(int index) {
        if (index < 0 || index >= size) {
            return -1;
        }
        ListNode* node = head;
        while (index != 0) {
            node = node->next;
            --index;
        }
        return node->val;
    }

    void addAtHead(int val) {
        ListNode* node = new ListNode(val);
        node->next = head;
        head = node;
        ++size;
    }

    void addAtTail(int val) {
        ListNode* node = new ListNode(val);
        if (size == 0) {
            head = node;
        } else {
            ListNode* prev = head;
            while (prev->next != nullptr) {
                prev = prev->next;
            }
            prev->next = node;
        }
        ++size;
    }

    void addAtIndex(int index, int val) {
        if (index < 0 || index > size) {
            return;
        }
        if (index == 0) {
            return addAtHead(val);
        }
        if (index == size) {
            return addAtTail(val);
        }
        // index > 0 && index < size
        ListNode* node = new ListNode(val);
        ListNode* prev = head;
        while (index != 1) {
            prev = prev->next;
            --index;
        }
        node->next = prev->next;
        prev->next = node;
        ++size;
    }

    void deleteAtIndex(int index) {
        if (index < 0 || index >= size) {
            return;
        }
        if (index == 0) {
            head = head->next;
        } else {
            ListNode* prev = head;
            ListNode* curr = prev->next;
            while (index != 1) {
                prev = curr;
                curr = curr->next;
                --index;
            }
            prev->next = curr->next;
        }
        --size;
    }
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```

# 206. 反转链表

[206. 反转链表 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-linked-list/description/)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev{};
        ListNode* curr{head};
        while (curr != nullptr) {
            ListNode* temp{curr->next};
            curr->next = prev;
            prev = curr;
            curr = temp;
        }
        return prev;
    }
};
```

时间复杂度: O(n)
空间复杂度: O(1)
