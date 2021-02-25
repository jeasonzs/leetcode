# Leetcode 题解 - 链表
<!-- GFM-TOC -->
* [Leetcode 题解 - 链表](#leetcode-题解---链表)
    * [1. 找出两个链表的交点](#1-找出两个链表的交点)
    * [2. 链表反转](#2-链表反转)
    * [3. 归并两个有序的链表](#3-归并两个有序的链表)
    * [4. 从有序链表中删除重复节点](#4-从有序链表中删除重复节点)
    * [5. 删除链表的倒数第 n 个节点](#5-删除链表的倒数第-n-个节点)
    * [6. 交换链表中的相邻结点](#6-交换链表中的相邻结点)
    * [7. 链表求和](#7-链表求和)
    * [8. 回文链表](#8-回文链表)
    * [9. 分隔链表](#9-分隔链表)
    * [10. 链表元素按奇偶聚集](#10-链表元素按奇偶聚集)
<!-- GFM-TOC -->


链表是空节点，或者有一个值和一个指向下一个链表的指针，因此很多链表问题可以用递归来处理。

##  1. 找出两个链表的交点

160\. Intersection of Two Linked Lists (Easy)

[Leetcode](https://leetcode.com/problems/intersection-of-two-linked-lists/description/) / [力扣](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/description/)

例如以下示例中 A 和 B 两个链表相交于 c1：

```html
A:          a1 → a2
                    ↘
                      c1 → c2 → c3
                    ↗
B:    b1 → b2 → b3
```

但是不会出现以下相交的情况，因为每个节点只有一个 next 指针，也就只能有一个后继节点，而以下示例中节点 c 有两个后继节点。

```html
A:          a1 → a2       d1 → d2
                    ↘  ↗
                      c
                    ↗  ↘
B:    b1 → b2 → b3        e1 → e2
```



要求时间复杂度为 O(N)，空间复杂度为 O(1)。如果不存在交点则返回 null。

设 A 的长度为 a + c，B 的长度为 b + c，其中 c 为尾部公共部分长度，可知 a + c + b = b + c + a。

当访问 A 链表的指针访问到链表尾部时，令它从链表 B 的头部开始访问链表 B；同样地，当访问 B 链表的指针访问到链表尾部时，令它从链表 A 的头部开始访问链表 A。这样就能控制访问 A 和 B 两个链表的指针能同时访问到交点。

如果不存在交点，那么 a + b = b + a，以下实现代码中 l1 和 l2 会同时为 null，从而退出循环。



如果只是判断是否存在交点，那么就是另一个问题，即 [编程之美 3.6]() 的问题。有两种解法：

- 把第一个链表的结尾连接到第二个链表的开头，看第二个链表是否存在环；
- 或者直接比较两个链表的最后一个节点是否相同。

```cpp
class Solution {
public:

    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
      auto p1 = headA, p2 = headB;
      while (p1 != p2) {
        p1 = (p1 == nullptr) ? headB : p1->next;
        p2 = (p2 == nullptr) ? headA : p2->next;
      }
      return p1;
    }
};
```

##  2. 链表反转

206\. Reverse Linked List (Easy)

[Leetcode](https://leetcode.com/problems/reverse-linked-list/description/) / [力扣](https://leetcode-cn.com/problems/reverse-linked-list/description/)

递归

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
      ListNode* prev = nullptr;
      while (head) {
        auto next = head->next;
        head->next = prev;
        prev = head;
        head = next;
      }
      return prev;
    }
};
```

##  3. 归并两个有序的链表

21\. Merge Two Sorted Lists (Easy)

[Leetcode](https://leetcode.com/problems/merge-two-sorted-lists/description/) / [力扣](https://leetcode-cn.com/problems/merge-two-sorted-lists/description/)

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
      ListNode node;
      auto p = &node;
      while (l1 && l2) {
        if (l1->val < l2->val) {
          p->next = l1;
          l1 = l1->next;
        } else {
          p->next = l2;
          l2 = l2->next;
        }
        p = p->next;
      }
      while (l1) {
        p->next = l1;
        l1 = l1->next;
        p = p->next;
      }
      while (l2) {
        p->next = l2;
        l2 = l2->next;
        p = p->next;
      }
      return node.next;
    }
};
```

##  4. 从有序链表中删除重复节点

83\. Remove Duplicates from Sorted List (Easy)

[Leetcode](https://leetcode.com/problems/remove-duplicates-from-sorted-list/description/) / [力扣](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/description/)

```html
Given 1->1->2, return 1->2.
Given 1->1->2->3->3, return 1->2->3.
```
```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
      if (!head) return head;
      auto prev = head, p = head->next;
      while (p) {
        if (p->val == prev->val) prev->next = p->next;
        else prev = p;
        p = p->next;
      }
      return head;
    }
};
```

##  5. 删除链表的倒数第 n 个节点

19\. Remove Nth Node From End of List (Medium)

[Leetcode](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/) / [力扣](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/description/)

```html
Given linked list: 1->2->3->4->5, and n = 2.
After removing the second node from the end, the linked list becomes 1->2->3->5.
```
```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
      auto l = head, r = head;
      while (n--) r = r->next;
      if (!r) return head->next;
      while (r->next) {
        l = l->next;
        r = r->next;
      }
      l->next = l->next->next;
      return head;
    }
};
```
##  6. 交换链表中的相邻结点

24\. Swap Nodes in Pairs (Medium)

[Leetcode](https://leetcode.com/problems/swap-nodes-in-pairs/description/) / [力扣](https://leetcode-cn.com/problems/swap-nodes-in-pairs/description/)

```html
Given 1->2->3->4, you should return the list as 2->1->4->3.
```

题目要求：不能修改结点的 val 值，O(1) 空间复杂度。
```cpp
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
      if (!head || !head->next) return head;
      auto first = head;
      auto second = head->next;
      first->next = swapPairs(second->next);
      second->next = first;
      return second;
    }
};
```
##  7. 链表求和

445\. Add Two Numbers II (Medium)

[Leetcode](https://leetcode.com/problems/add-two-numbers-ii/description/) / [力扣](https://leetcode-cn.com/problems/add-two-numbers-ii/description/)

```html
Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 8 -> 0 -> 7
```

题目要求：不能修改原始链表。
```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
      stack<int> s1, s2;
      while (l1) {
        s1.push(l1->val);
        l1 = l1->next;
      }
      while (l2) {
        s2.push(l2->val);
        l2 = l2->next;
      }
      int c = 0;
      ListNode* p = nullptr;
      while (!s1.empty() || !s2.empty() || c) {
        if (!s1.empty()) {
          c += s1.top();
          s1.pop();
        }
        if (!s2.empty()) {
          c += s2.top();
          s2.pop();
        }
        auto now = new ListNode(c % 10);
        now->next = p;
        p = now;
        c /= 10;
      }
      return p;
    }
};
```

##  8. 回文链表

234\. Palindrome Linked List (Easy)

[Leetcode](https://leetcode.com/problems/palindrome-linked-list/description/) / [力扣](https://leetcode-cn.com/problems/palindrome-linked-list/description/)

题目要求：以 O(1) 的空间复杂度来求解。

切成两半，把后半段反转，然后比较两半是否相等。
```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
      vector<int> vec;
      while (head) {
        vec.push_back(head->val);
        head = head->next;
      }
      int l = 0, r = vec.size() - 1;
      while (l <= r) {
        if (vec[l++] != vec[r--]) return false;
      }
      return true;
    }
};
```

##  9. 分隔链表

725\. Split Linked List in Parts(Medium)

[Leetcode](https://leetcode.com/problems/split-linked-list-in-parts/description/) / [力扣](https://leetcode-cn.com/problems/split-linked-list-in-parts/description/)

```html
Input:
root = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10], k = 3
Output: [[1, 2, 3, 4], [5, 6, 7], [8, 9, 10]]
Explanation:
The input has been split into consecutive parts with size difference at most 1, and earlier parts are a larger size than the later parts.
```

题目描述：把链表分隔成 k 部分，每部分的长度都应该尽可能相同，排在前面的长度应该大于等于后面的。


##  10. 链表元素按奇偶聚集

328\. Odd Even Linked List (Medium)

[Leetcode](https://leetcode.com/problems/odd-even-linked-list/description/) / [力扣](https://leetcode-cn.com/problems/odd-even-linked-list/description/)

```html
Example:
Given 1->2->3->4->5->NULL,
return 1->3->5->2->4->NULL.
```