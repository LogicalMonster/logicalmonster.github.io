---
title: 算法训练营Day4
date: 2025-06-01 21:13:30
tags:
---
# 24. 两两交换链表中的节点

[24. 两两交换链表中的节点 - 力扣（LeetCode）](https://leetcode.cn/problems/swap-nodes-in-pairs/description/)

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
    ListNode* swapPairs(ListNode* head) {
        size_t i{};
        ListNode* prev{};
        ListNode* curr{head};
        while (curr != nullptr) {
            ListNode* temp{curr->next};
            if (i % 2 == 0) {
                if (temp != nullptr) {
                    if (i == 0) {
                        head = temp;
                    } else {
                        prev->next = temp;
                    }
                }
                prev = curr;
            } else {
                prev->next = curr->next;
                curr->next = prev;
            }
            curr = temp;
            ++i;
        }
        return head;
    }
};
```

时间复杂度：O(n)
空间复杂度：O(1)

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
    ListNode* swapPairs(ListNode* head) {
        ListNode dummy(0);
        dummy.next = head;
        ListNode* prev{&dummy};
        ListNode* curr{prev->next};
        while (curr && curr->next) {
            ListNode* node{curr->next};
            curr->next = node->next;
            node->next = curr;
            prev->next = node;
            prev = curr;
            curr = curr->next;
        }
        return dummy.next;
    }
};
```

时间复杂度：O(n)
空间复杂度：O(1)

# 19. 删除链表的倒数第N个节点

[19. 删除链表的倒数第 N 个结点 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* prev{nullptr};
        ListNode* curr{head};
        while (true) {
            ListNode* node{curr};
            for (size_t i = 0; i < n; ++i) {
                if (node == nullptr) {
                    return head;
                }
                node = node->next;
            }
            if (node == nullptr) {
                break;
            }
            prev = curr;
            curr = curr->next;
        }
        if (prev == nullptr) {
            head = curr->next;
        } else {
            prev->next = curr->next;
        }
        return head;
    }
};
```

时间复杂度: O(L * n)
空间复杂度: O(1)

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* prev{nullptr};
        ListNode* curr{head};
        ListNode* node{curr};
        for (size_t i = 0; i < n; ++i) {
            if (node == nullptr) {
                return head;
            }
            node = node->next;
        }
        while (node != nullptr) {
            node = node->next;
            prev = curr;
            curr = curr->next;
        }
        if (prev == nullptr) {
            head = curr->next;
        } else {
            prev->next = curr->next;
        }
        return head;
    }
};
```

时间复杂度: O(L)
空间复杂度: O(1)

# 面试题 02.07. 链表相交

[面试题 02.07. 链表相交 - 力扣（LeetCode）](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/description/)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if (headA == nullptr || headB == nullptr) {
            return nullptr;
        }
        ListNode *nodeA{headA};
        ListNode *nodeB{headB};
        while (nodeA != nodeB) {
            nodeA = nodeA == nullptr ? headB : nodeA->next;
            nodeB = nodeB == nullptr ? headA : nodeB->next;
        }
        return nodeA;
    }
};
```

时间复杂度：O(m + n)
空间复杂度：O(1)

# 142. 环形链表II

[142. 环形链表 II - 力扣（LeetCode）](https://leetcode.cn/problems/linked-list-cycle-ii/description/)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* detectCycle(ListNode* head) {
        ListNode* slow{head};
        ListNode* fast{head};
        while (true) {
            if (fast == nullptr || fast->next == nullptr) {
                return nullptr;
            }
            slow = slow->next;
            fast = fast->next->next;
            if (slow == fast) {
                break;
            }
        }
        ListNode* node{head};
        while (node != slow) {
            node = node->next;
            slow = slow->next;
        }
        return node;
    }
};
```

时间复杂度: O(n)
空间复杂度: O(1)
