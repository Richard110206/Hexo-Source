---
title: Leetcode Mistake Collection 2
date: 2025-09-10 12:47:08
tags: [Leetcode,updating]
index_img: https://github.com/Richard110206/Blog-image/blob/main/cover/Leetcode-Mistake-Collection.png?raw=true
category: Leetcode Mistake Collection
category_bar: true
description: The article is intended to systematically document the errors I encountered while solving LeetCode problems, along with the corresponding corrective strategies.
math: true
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

- 若两链表**相交**
$$length.A=a+m$$$$length.B=b+m$$

其中a、b分别为**相交前分别独有长度**，m为**两链表相交的长度**对于指针A、B遍历的长度相等：$$a+m+b=b+m+a$$

此时两指针**恰好相遇于两链表交点**

- 若两链表**不相交**
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
***
## Leetcode 21. 合并两个有序链表

[原题链接](https://leetcode.cn/problems/merge-two-sorted-lists)
{%fold into @基础解法 %}
```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode *dummy=new ListNode(0);
        ListNode * curr=dummy;
        ListNode *p1=list1,*p2=list2;
        while(p1!=nullptr && p2!=nullptr){
            if(p1->val<=p2->val){
                curr->next=p1;
                p1=p1->next;
            }
            else{
                curr->next=p2;
                p2=p2->next;
            }
            curr=curr->next;
        }
        if(p1==nullptr){
            curr->next=p2;
        }
        else curr->next=p1;
        return dummy->next;
    }
};
```
### Tips : 哑结点
在链表操作中，很多操作都依赖于**当前节点的前驱节点**来完成，而头节点没有前驱节点，因此需要特殊处理。

{%note info%}
以删除一个节点为例：
- 对于非头节点：只需找到它的前驱节点`prev`，执行`prev->next = prev->next->next`即可。
- 对于头节点：由于没有前驱，只能直接修改头指针（`head = head->next`）。

当我们尝试使用对头节点和普通节点**统一逻辑**进行删除：
```cpp
ListNode* removeElements(ListNode* head, int val) {
    ListNode* current = head;
    
    while (current != nullptr) {
        // 尝试用统一逻辑：检查下一个节点
        if (current->next != nullptr && current->next->val == val) {
            current->next = current->next->next; // 删除下一个节点
        } else {
            current = current->next;
        }
    }
    return head; // 问题：如果头节点要被删除，这里无法处理！
}
```
#### 方法一：常规方法
采用头节点特殊的显式处理：
```cpp
ListNode* removeElements(ListNode* head, int val) {
    // 特别处理1：先处理所有需要删除的头节点
    while (head != nullptr && head->val == val) {
        head = head->next; // 直接移动头指针
    }
    
    // 特别处理2：如果链表为空，直接返回
    if (head == nullptr) return nullptr;
    
    // 然后用统一逻辑处理后续节点
    ListNode* current = head;
    while (current->next != nullptr) {
        if (current->next->val == val) {
            current->next = current->next->next;
        } else {
            current = current->next;
        }
    }
    
    return head;
}
```
#### 方法二：使用哨兵节点（统一逻辑）：
```cpp
ListNode* removeElements(ListNode* head, int val) {
    // 创建哨兵节点，让头节点也有"前驱"
    ListNode* dummy = new ListNode(0);
    dummy->next = head;
    
    ListNode* current = dummy;
    
    // 现在可以用统一逻辑处理所有节点
    while (current->next != nullptr) {
        if (current->next->val == val) {
            current->next = current->next->next;
        } else {
            current = current->next;
        }
    }
    
    return dummy->next; // 返回新的头节点
}
```
{%endnote%}

这时我们就可以采用**哑结点**来简化边界情况的处理，哑结点是一个不存储实际数据的**特殊节点**，通常作为链表的"**虚拟头节点**" 存在，它的**next指针指向真正的头节点**：

```cpp
ListNode *dummy=new ListNode(0);  //这里初始化取值无意义
dummy->next=head;
 ListNode* result = dummy->next;
delete dummy; // 释放哑节点的内存
return result;
```
{%note danger%}
:warning: 注意事项:
- 哑结点是**临时的辅助节点**，最后需要**释放内存**（避免内存泄漏）
- 操作完成后，**真正的头节点**是`dummy->next`    
{%endnote%}

{%endfold%}

### 方法一：递归
```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        if(list1==nullptr){
            return list2;
        }
        else if(list2==nullptr){
            return list1;
        }
        else if(list1->val<list2->val){
            list1->next=mergeTwoLists(list1->next,list2);
            return list1;
        }
        else{
            list2->next=mergeTwoLists(list1,list2->next);
            return list2;
        }
    }
};
```
### 方法二：迭代（本质是常规解法）
```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode *prehead=new ListNode(-1);
        ListNode *pre=prehead;
        while(list1!=nullptr && list2!=nullptr){
            if(list1->val<=list2->val){
                pre->next=list1;
                list1=list1->next;
            }
            else {
                pre->next=list2;
                list2=list2->next;
            }
            pre=pre->next;
        }
        pre->next=list1==nullptr?list2:list1;
        return prehead->next;
    }
};
```

***
## Leetcode 53. 最大子数组和
[原题链接](https://leetcode.cn/problems/maximum-subarray)
{%fold into @常规解法Time Error %}
```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n=nums.size();
        int max=-100000;
        for(int i=0;i<n;i++){
            int sum=0;
            for(int j=i;j<n;j++){
                sum+=nums[j];
                if(sum>max){
                    max=sum;
                }
            }
        }
        return max;
    }
};
```
{%endfold%}

### 方法一：动态规划
我们维护一个函数`f(i)`，表示以第`i`个数结尾的**最大子数组和**，那么显然我们就是要求`f(i)`的最大值：
$$max_{i=1}^n f(i)$$

而`f(i)`仅仅与`f(i-1)`有关，取`f(i-1)+nums[i]`和`nums[i]`中的最大值，写出动态规划状态转移方程，即：
$$f(i)=max(f(i-1)+nums[i],nums[i])$$

然而还可以进行优化，我们无需显式的表示出`f(i)`，采用“滚动数组”的思想，只需要用一个变量`pre`来维护前`i-1`个`f(x)`的最大值即可。
```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxAns=nums[0],pre=0;
        for(const auto& it:nums){
            pre=max(pre+it,it);
            maxAns=max(maxAns,pre);
        }
        return maxAns;
    }
};
```
