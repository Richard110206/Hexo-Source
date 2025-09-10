---
title: Leetcode Mistake Collection 2
date: 2025-09-10 12:47:08
tags: [Leetcode,updating]
index_img: https://github.com/Richard110206/Blog-image/blob/main/cover/Leetcode-Mistake-Collection.png?raw=true
category: Leetcode Mistake Collection
category_bar: true
description: The article is intended to systematically document the errors I encountered while solving LeetCode problems, along with the corresponding corrective strategies.
---

## Leetcode 160. 相交链表

[原题链接](https://leetcode.cn/problems/intersection-of-two-linked-lists?envType=problem-list-v2&envId=hash-table)


### 方法一：哈希表

遍历ListNodeA，用哈希表存**储链表节点**，再遍历ListNodeB，如果ListNodeB的节点在哈希表中，则返回该节点。
```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        unordered_set <ListNode *> set;
        while(headA!=nullptr){
            set.insert(headA);
            headA=headA->next;
        }
        while(headB!=nullptr){
            if(set.find(headB)!=set.end()){
                return headB;
            }
            headB=headB->next;
        }
        return nullptr;
    }
};
```
### 方法二：双指针

定义两个指针分别指向头节点，同时遍历向后进行遍历，若遍历为nullptr，则将指针指向另一条链表的头节点，直至两指针相遇，或者两个指针都为nullptr。

- 若两链表相交
   - length.A=a+m
   - length.B=b+m

(其中m为两链表相交的长度)

   - 对于A指针遍历长度为：a+m+b；
   - 对于B指针遍历长度为：b+m+a;

此时恰好相遇于两链表交点

- 若两链表不相交
则两指针将AB均遍历结束，同时指向nullptr。
```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *pA=headA, *pB=headB;
        while(pA!=pB){
            pA=pA==nullptr?headB:pA->next;
            pB=pB==nullptr?headA:pB->next;
        }
        return pA;
    }
};
```
