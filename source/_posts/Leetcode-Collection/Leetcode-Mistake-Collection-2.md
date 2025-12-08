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

[Leetcode 160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists?envType=problem-list-v2&envId=hash-table)


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

[Leetcode 21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists)
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
[Leetcode 53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray)
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

## Leetcode 739.每日温度
[Leetcode 739. 每日温度](https://leetcode.cn/problems/daily-temperatures)
{%fold into @Time Error Version :x: %}
双重循环暴力搜索时间复杂度为$O(n^2)$，会超时。
```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        int n=temperatures.size();
        vector<int> ans;
        for(int i=0;i<n-1;i++){
            for(int j=i+1;j<n;j++){
                if(temperatures[i]<temperatures[j]){
                    ans.push_back(j-i);
                    break;
                }
                if(j==n-1){
                ans.push_back(0);
                }
            }
        }
        ans.push_back(0);
        return ans;
    }
};
```
{%endfold%}
### 方法一：暴力 2.0
&emsp;&emsp;考虑到正向暴力解法中，每个天数都需要**遍历其后面的所有数字**，时间复杂度过高；而题给温度在$[30,100]$范围内，我们不妨维护一个数组`posttem`初始化为`INT_MAX`，对数组从后向前遍历，在遍历过程中更新**每个温度出现的最早索引**。

&emsp;&emsp;反向遍历温度列表。对于每个元素`temperatures[i]`，在数组`posttem`中找到从`temperatures[i] + 1`到`100`中每个温度第一次出现的下标，将其中的最小下标记为`warmer`，则`warmer`为下一次温度比当天高的下标。如果`warmer`不为无穷大，则`warmer - i`即为下一次温度比当天高的等待天数，最后更新令`posttem[temperatures[i]] = i`。

```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        int n=temperatures.size();
        vector <int> posttem(101,INT_MAX);
        vector <int> ans(n);
        for(int i=n-1;i>=0;i--){
            int warmer=INT_MAX;
            for(int j=temperatures[i]+1;j<=100;j++){
                warmer=min(warmer,posttem[j]);
            }
            if(warmer!=INT_MAX){
                ans[i]=warmer-i;
            }
            posttem[temperatures[i]]=i;
        }
        return ans;
    }
};
```
### 方法二：单调栈 
```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        int n=temperatures.size();
        stack <int> st;
        vector <int> ans(n);
        for(int i=0;i<n;i++){
            while(!st.empty()&&temperatures[i]>temperatures[st.top()]){
                int pre=st.top();
                ans[pre]=i-pre;
                st.pop();
            }
            st.push(i);
        }
        return ans;
    }
};
```

### 补充：单调栈

{%note info%}
&emsp;&emsp;单调栈是一种在普通栈的基础上增加了 “**维护单调性**” 的核心规则的**特殊栈结构**——在元素入栈时，通过弹出栈顶不满足单调性的元素，**确保栈内元素始终有序**。
{%endnote%}

&emsp;&emsp;例如，在本题中，我们需要找到每个元素的下一个更大元素，即对于每个元素`temperatures[i]`，找到第一个比它大的元素`temperatures[j]`，其中`i < j`。我们维护一个**存储下标的单调栈**，从栈底到栈顶的下标对应的温度列表中的温度依次递减。如果一个下标在单调栈里，则表示尚未找到下一次温度更高的下标。正向遍历温度列表：

- 对于每个元素`temperatures[i]`，如果栈为空，则直接将`i`进栈，
- 如果栈不为空，则比较栈顶元素`pre`对应的温度`temperatures[pre]`和当前温度`temperatures[i]`，
   
   - 如果`temperatures[i]>temperatures[pre]`，则将`pre`移除，并将`pre`对应的等待天数赋为`i - pre`，重复上述操作直到栈为空或者栈顶元素对应的温度大于等于当前温度，然后将`i`进栈。

#### 适用场景：
- 寻找每个元素的 **“下一个更小/大元素”** [LeetCode 42.接雨水](https://leetcode.cn/problems/trapping-rain-water)


{%fold into@十一集训预选赛 A 求和%}
### 题目描述
给定 $n$ 个整数 $ a_1, a_2, \cdots, a_n $，求它们两两相乘再相加的和，即
$S = a_1 \cdot a_2 + a_1 \cdot a_3 + \cdots + a_1 \cdot a_n + a_2 \cdot a_3 + a_2 \cdot a_4 + \cdots + a_2 \cdot a_n + \cdots + a_{n - 2} \cdot a_{n - 1} + a_{n - 2} \cdot a_n + a_{n - 1} \cdot a_n$
### 输入

输入的第一行包含一个整数 $n$。
第二行包含 $n$ 个整数 $ a_1, a_2, \cdots, a_n $。

### 输出
输出一个整数 $S$，表示所求的和。请使用合适的数据类型进行运算。

### 样例输入
```
4
1 3 6 9
```
### 样例输出
```
117
```
{%endfold%}

#### 利用数学公式进行优化
$(a_1 + a_2 + \cdots + a_n)^2 $
$= a_1^2 + a_2^2 + \cdots + a_n^2 + 2\left(a_1a_2 + a_1a_3 + \cdots + a_{n-1}a_n\right)$


## Leetcode 209. 长度最小的子数组
[Leetcode 209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum)

### 方法一：前缀和+二分查找

&emsp;&emsp;我们额外创建一个数组 `sums` 用于存储数组 `nums` 的前缀和，其中 `sums[i]` 表示从 `nums[0]` 到 `nums[i−1]` 的元素和。得到前缀和之后，对于每个开始下标 `i`，可通过二分查找得到大于或等于 `i` 的最小下标 `bound`，使得 `sums[bound]−sums[i−1]≥s`，并更新子数组的最小长度（此时子数组的长度是 `bound−(i−1)`）。

{%note danger%}
因为这道题保证了数组中每个元素都为正，所以**前缀和一定是递增的**，这一点保证了二分的正确性。如果题目没有说明数组中每个元素都为正，这里就不能使用二分来查找这个位置了。
{%endnote%}

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int n=nums.size();
        int ans=INT_MAX;
        vector<int> sums(n+1);
        for(int i=1;i<n+1;i++){
            sums[i]=nums[i-1]+sums[i-1];
        }
        for(int i=1;i<n+1;i++){
        int search=target+sums[i-1];
        auto bound=lower_bound(sums.begin(),sums.end(),search);
        if(bound!=sums.end()){
            ans=min(ans,static_cast<int>((bound - sums.begin()) - (i - 1)));
           }
        }
        return ans==INT_MAX?0:ans;
    }
};
```
#### 函数
`lower_bound`函数是`<algorithm>`头文件中的一个函数，用于在**有序数组**中查找（**二分查找**）第一个**大于等于**指定值的元素的迭代器。
- 找到第一个**大于等于**目标值 `search` 的元素的位置（迭代器）。
- 如果序列中所有元素都小于 `search`，则返回**末尾迭代器**（如 sums.end()）

`lower_bound(first, last, value)`
- first：序列的**起始迭代器**
- last：序列的**结束迭代器**
- value：要查找的**目标值**

相关的同源函数：
- `upper_bound` **大于（无等于）目标值**的第一个元素的迭代器
- `equal_bound` **等于目标值**的第一个元素的迭代器

若是降序排列的则需要**手动传入比较器**：
```cpp
// 降序序列中查找
auto it = lower_bound(begin, end, value, greater<int>());
```

### 补充：前缀和
&emsp;&emsp;前缀和指的是数组中从第一个元素到当前元素之间所有元素的累加和。具体来说，给定一个数组`nums`，它的前缀和数组`prefix`定义为：`prefix[i]=nums[0]+nums[1]+…+nums[i-1]`，通过前缀和我们可以轻松计算任意连续子区间的和，如`prefix[i]-prefix[j]`。
{%fold into@前缀和练习%}
[Leetcode 3076. 数组中的最大字符串值](https://leetcode.cn/problems/taking-maximum-energy-from-the-mystic-dungeon?envType=daily-question&envId=2025-10-10)

由于末尾的`n-k`为循环终点，必会被取到，因而采用逆序遍历的“前缀和”
```cpp
class Solution {
public:
    int maximumEnergy(vector<int>& energy, int k) {
        int n=energy.size(),ans=INT_MIN;
        int sum;
        for(int i=n-k;i<n;i++){
            sum=0;
            for(int j=i;j>=0;j-=k){
                sum+=energy[j];
                ans=max(ans,sum);
            }
        }
        return ans;
    }
};
```
{%endfold%}


### 方法二：滑动窗口
&emsp;&emsp;我们使用两个指针 `start` 和 `end` 表示当前子数组的开始位置和结束位置，`sum` 表示当前子数组的和。初始时，`start` 和 `end` 都指向数组的第一个元素，`sum` 为 0。
- 当`sum < target`时，将 `end` 向后移动一位，`sum`加上`nums[end]`
- 当`sum >= target`时，将 `start` 向后移动一位，`sum`减去`nums[start]`，并更新最短子数组
&emsp;&emsp;
```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int n=nums.size();
        int start=0,end=0;
        int sum=0;
        int ans=INT_MAX;
        while(end<n){
            sum+=nums[end];
            while(sum>=target){
                ans=min(ans,end-start+1);
                sum-=nums[start];
                start++;
            }
            end++;
        }
        return ans==INT_MAX?0:ans;
    }
};
```

{%note danger%}
补充：注意前缀和的使用场景(有些题目用前缀和+滑动窗口也要注意数组是否恒为正数y)
{%endnote%}
## Leetcode.560 和为k的子数组
[Leetcode.560 和为k的子数组](https://leetcode.cn/problems/subarray-sum-equals-k)

{%fold into@ Wrong Version%}
```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int left=0,right=0;
        int n=nums.size();
        int sum=nums[0],count=0;
        while(right!=n){
            if(sum>k) {
                if(right==left){
                    left++;
                    right++;
                    sum=nums[left];
                }
                sum-=nums[left];
                left++;}
            else if(sum<k) {
                right++;
                sum+=nums[right];
            }
            else {
                count++;
                sum-=nums[left];
                left++;}
        }
        return count;
    }
};
```
{%endfold%}
### 方法一：哈希表+前缀和
初始化哈希表记录前缀和0出现1次（对应从数组开头开始的子数组）。遍历每个数时累加得到当前前缀和，检查 当前`前缀和 - k` 是否在哈希表中，如果在则累加其出现次数到结果中，最后将当前前缀和加入哈希表并更新出现次数。(需要查找用`unordered_map`时间复杂度为 $O(1)$，不需要有序的`map`，效率更高)
```
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int count=0;
        int sum=0;
        unordered_map<int,int> mp;
        mp[0]=1;
        for(int num:nums){
            sum+=num;
            if(mp.find(sum-k)!=mp.end()){
                count+=mp[sum-k];
            }
            mp[sum]++;
        }
        return count;
    }
};
```