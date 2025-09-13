---
title: Double Pointer
date: 2025-08-29 01:10:42
tags: [Algorithm, Double Pointer]
index_img: https://github.com/Richard110206/Blog-image/blob/main/cover/DoublePointer.png?raw=true
category: Algorithm
category_bar: true
description: Focusing on the three kinds of double-pointer techniques，fast and slow, left and right, and sliding windows.
---

## Double Pointer
**双指针**是算法设计中一种**高效且灵活**的技巧，通过两个指针在数据结构中**协同移动**，能够将原本需要**嵌套循环** $ O(n^2) $ 的问题优化为线性时间复杂度 $O(n) $，同时减少空间消耗。

### 左右指针

- **核心思想**：两个指针分别从**序列的两端**出发，**向中间移动**，通过判断条件调整指针位置，**直至相遇或满足特定条件**。
- **适用场景**：
  
  - 有序数组问题（如两数之和、三数之和）
  - 对称结构判断（如回文串、回文链表）
  - 二分查找变种（如寻找旋转数组的最小值）
  - 需要从两端向中间收缩的场景（如盛最多水的容器）

- **方法**：
  - 双指针分别**指向数组两端**：左指针`left = 0`（**起点**），右指针`right = n-1`（**终点**）
  - 循环条件：`left < right`（指针**未相遇**）
  - 根据要求**动态移动指针**

[Leetcode 167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted)
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int left=0,right=numbers.size()-1;
        while(right>left){
            if(numbers[right]+numbers[left]>target){
                right--;
            }
            else if(numbers[right]+numbers[left]<target){
                left++;
            }
            else break;
        }
        return {left+1,right+1};
    }
};
```

***

[Leetcode 11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water)
```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int left=0,right=height.size()-1;
        int max_vol=INT_MIN;
        int vol;
        while(right!=left){
            vol=(right-left)*min(height[left],height[right]);
            max_vol=max(max_vol,vol);
            if(height[left]>height[right]){
                right--;
            }
            else left++;
        }
        return max_vol;
    }
};
```

***

[Leetcode 27. 移除元素](https://leetcode.cn/problems/remove-element)
#### 方法一：类快慢指针
让我们**模拟**一下程序运行的过程：
- 当前几个均`!=val`时：`left`和`right`都**指向同一个索引**，**同时向后移动**
- 当同时指向`val`时，`right`向后继续移动寻找`!=val`，`left`保持不变，标记`val`的位置
- 当`right`再次指向`!=val`时，这时`left`的**标记起作用**了，与`nums[right]`进行交换并向后移动
```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int right,left=0;
        int n=nums.size();
        for(right=0;right<n;right++){
            if(nums[right]!=val){
                nums[left]=nums[right];
                left++;
            }
        }
        return left;
    }
};
```
#### 方法二：对撞指针
```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int n=nums.size();
        int right=n-1,left=0;
        while(left<=right){
            if(nums[left]==val){
                swap(nums[right],nums[left]);
                right--;
            }else {
                left++;
            }
        }
        return left;
    }
};
```

***

### 快慢指针

- **核心思想**：两个指针从**同一起点出发**，以**不同速度同向移动**，利用**速度差**解决问题。
- **适用场景**：
  
  - 链表问题（如检测环、寻找环入口、链表中点）
  - 数组去重（如删除有序数组中的重复项）
  - 追及问题（如找到链表中倒数第 k 个节点）

- **方法**：
  - 快指针`fast`和慢指针`slow`均**指向起点**（如链表头节点、数组下标 0）
  - **快指针每次移动 2 步**（`fast = fast->next->next`）
  - **慢指针每次移动 1 步**（`slow = slow->next`）
  - 直至**快指针到达终点**（`fast == nullptr`或`fast->next == nullptr`）或**两指针相遇**

{%fold into @ 有环必相遇%}
若链表存在环，那么环是**闭合的循环结构**。一旦指针进入环，就会在环内**永不停歇地循环移动**（因为环的末尾节点指向环内某个节点，而非`nullptr`）。
{%endfold%}

***

[Leetcode 141.环形链表](https://leetcode.cn/problems/linked-list-cycle)
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
    bool hasCycle(ListNode *head) {
        if(head==nullptr || head->next==nullptr){
            return false;//空链表或单节点无环
        }
        ListNode* fast=head->next;
        //初始化，防止误判（直接跳过循环）
        ListNode* slow=head;
        while(slow!=fast){
            if(fast == nullptr || fast->next == nullptr){
                return false;
            }
            fast=fast->next->next;
            slow=slow->next;
        }
        return true;
    }
};
```

***
 
[Leetcode 26. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array)

- 使用`fast`遍历数组，`slow`前段是**已处理序列**，指向的是下一个替换元素的位置（`tag`）
  - 若`fast`标记重复元素，`slow`不动
  - 若`fast`标记非重复元素，`slow`交换位置
```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int fast=1,slow=1;
        int n=nums.size();
        while(fast<n){
            if(nums[fast]!=nums[fast-1]){
                nums[slow]=nums[fast];
                slow++;
            }
            fast++;
        }
        return slow;
    }
};
```

***

### 滑动窗口

- **核心思想**：维护一个由**左右指针界定的「窗口」**（**连续区间**），通过移动左右指针**动态调整窗口范围**，以寻找满足条件的最优子序列。
- **适用场景**：

  - 子串 / 子数组问题（如最小覆盖子串、最长无重复子串）
  - 区间求和问题（如长度最小的子数组）
  - 固定长度区间问题（如滑动窗口最大值）

- **方法**：
  - 使用两个指针`left=0,right=0`定义一个窗口，**初始时窗口为空**。
  - 移动右指针`right++`**扩展窗口**，移动左指针`left++`**缩小窗口**，直到满足条件。
  - 在收缩过程中**更新满足条件的最优解**

基本模版：
```cpp
//外层循环扩展右边界，内层循环扩展左边界
for (int l = 0, r = 0 ; r < n ; r++) {
	//当前考虑的元素
	while (l <= r && check()) {//区间[left,right]不符合题意
        //扩展左边界
    }
    //区间[left,right]符合题意，统计相关信息
}
```

***

[Leetcode 76. 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring)

```cpp
class Solution {
public:
    unordered_map <char, int> ori, cnt;

    bool check() {
        for (const auto &p: ori) {
            if (cnt[p.first] < p.second) {
                return false;
            }
        }
        return true;
    }

    string minWindow(string s, string t) {
        for (const auto &c: t) {
            ++ori[c];
        }

        int l = 0, r = -1;
        int len = INT_MAX, ansL = -1, ansR = -1;

        while (r < int(s.size())) {
            if (ori.find(s[++r]) != ori.end()) {
                ++cnt[s[r]];
            }
            while (check() && l <= r) {
                if (r - l + 1 < len) {
                    len = r - l + 1;
                    ansL = l;
                }
                if (ori.find(s[l]) != ori.end()) {
                    --cnt[s[l]];
                }
                ++l;
            }
        }

        return ansL == -1 ? string() : s.substr(ansL, len);
    }
};
```

[Leetcode 3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters)

#### 滑动窗口+哈希表
维持两个指针`right``left`，两指针之间代表**不重复子序列**，当`left++`时，会发现终止点`right`的**索引是递增的**，通过滚动数组记录最大值与当前值进行比较，用哈希表记录已在子序列中的字符。

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_set<char> map;
        int n=s.length();
        int right=-1;
        int ans=0;
        for(int i=0;i<n;i++){
            if(i!=0){
            map.erase(s[i-1]);
            }
            while(right<n-1 && !map.count(s[right+1])){
                map.insert(s[right+1]);
                right++;
            }
            ans=max(ans,right-i+1);
        }
        return ans;
    }
};
```
