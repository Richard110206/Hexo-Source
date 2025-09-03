---
title: Array and Linkedlist
date: 2025-08-22 16:16:33
tags: [datastructure,linkedlist]
index_img: https://raw.githubusercontent.com/Richard110206/Blog-image/refs/heads/main/cover/linkedlist.webp
category: Data Structure
category_bar: true
description: Reviewing the chapter about arrays and linked lists in Data Structure
---

# Introduction
&emsp;&emsp;博主上学期结束了数据结构的学习，但是由于高数、线代、离散等课程积压，课程进度过快，数据结构学的粗枝大叶，很多地方蜻蜓点水般一笔带过，甚至是理论课学思想，实践课STL直接上，很多地方没有仔细钻研，考虑到下学期**程序综合实践有“CCF-CSP认证”**，以及数据结构 && 算法的重要性，遂打算再次进行学习相关内容，但相对的会有详略取舍！


参考教材：[Hello,算法！](https://github.com/krahets/hello-algo)

## Physical Structure

&emsp;&emsp;内存是**所有程序的共享资源**，当某块内存被某个程序占用时，则无法被其他程序同时使用。就正如我们平时买电子产品时，讨论的8G、16G、32G的内存，当你同时打开开多个程序时就可能发生卡顿的现象。因此在数据结构和算法设计中，**内存资源**是一个很重要的考虑因素。

{%note info%}
比如，算法所占用的内存峰值不应超过系统剩余空闲内存；如果**缺少连续大块的内存空间**，那么所选用的数据结构必须能够**存储在分散的内存空间**里！
{%endnote%}

所有的数据结构都是基于**数组**（**连续空间存储**）、**链表**（**分散空间存储**）或二者的组合实现的！

- 基于**数组**可实现：栈、队列、哈希表、树、堆、图、矩阵、张量等
- 基于**链表**可实现：栈、队列、哈希表、树、堆、图、等


## Array

![数组地址计算](https://github.com/Richard110206/Blog-image/blob/main/article/DataStructure/Address-Caculate.png?raw=true)

- 数组的索引本质上是**内存地址的偏移量**

### Array in STL
```cpp
#include <vector>
/*动态数组的初始化*/

/*初始化int数组*/
vector<int> vec;					//定义一个int型向量
vector<int> vec(10);				//vec有10个值为0的元素
vector<int> vec(10, 1);				//vec初始有10个值为1的元素
vector<vector<int>> vec;			//定义一个int型二维向量
vector<int>::iterator it;			//定义一个int型迭代器

/*初始化string数组*/
vector<string> vec(10, "string")    //vec初始有10个值为"string"的元素
vector<string> vec(a.begin(), a.end());	//vec是a的复制

/*vector基本用法*/
vec.push_back();						//在数组的尾部添加一个元素
vec.pop_back();							//去掉数组的最后一个元素
vec.begin();							//得到数组头的指针，用迭代器接受
vec.end();								//得到数组的最后一个元素+1的指针
vec.clear();							//清空容器中所有元素
vec.empty();							//判断容器是否为空
vec.erase(vec.begin() + i, vec.end() + j);	//删除[i,j)区间的元素
vec.erase(vec.begin() + i);				//删除第i+1个元素
vec.insert(vec.begin() + i, a);			//在第i+1个元素前面插入a
vec.insert(vec.end(), 10, a);			//尾部插入10个值为a的元素
vec.insert(pos, data);					//在pos处插入数据
vec.size();								//返回容器中实际元素的个数
vec.back();								//返回尾部元素
vec.front();							//返回头部元素
vec.resize(n);							//数组大小变为n
reverse(vec.begin(), vec.end())			//用函数reverse()翻转数组
sort(vec.begin(), vec.end())			//用函数排序，默认从小排到大
```

### Pros and Cons

{%note info%}
- 空间效率高：为数据分配连续内存块，无需额外结构开销
- 支持随机访问：循序在O(1) 时间内访问任意元素
- 缓存局部性：当访问数组元素时，计算机还会缓存器周围的其他数据，从而借助高速缓存来提升后续操作的执行速度
{%endnote%}

{%note info%}
- 插入与删除效率低：元素较多时，需要移动大量元素
- 长度不可变：在初始化后长度就固定了。扩容数组需要将所有数据复制到新数组，开销很大
- 空间浪费：如果分配的大小超过实际所需，那么多余的空间就被浪费了
{%endnote%}

## Linkedlist
由前面的内存空间可知，在一个复杂的系统运行环境下，空闲的内存空间可能**散落在内存各处**，而存储数组的内存空间必须是**连续的**，当数组非常大时，内存可能无法提供如此大的连续空间，那么此时**链表的灵活性**就体现出来了！

![链表定义与存储方式](https://github.com/Richard110206/Blog-image/blob/main/article/DataStructure/Physical-Storage-Address.png?raw=true)

链表的组成单位是**节点**（node）对象。每个节点都包含两项数据：

- **节点值**
- **指向下一节点的指针**

链表的首个节点被称为“**头节点**”，最后一个节点被称为“**尾节点**”。
```c++
struct Node {
    int val;         // 节点值
    Node* next;  // 指向下一节点的指针
    Node(int x) : val(x), next(nullptr) {}  // 构造函数
};
```
链表节点 `Node` 除了包含值，还需额外保存**一个指针**。因此在相同数据量下，**链表比数组占用更多的内存空间**。

### 链表操作
#### 初始化链表

```cpp
// 实现1->3->2->5->4的链表
Node* n0=new Node(1);
Node* n1=new Node(3);
Node* n2=new Node(2);
Node* n3=new Node(5);
Node* n4=new Node(4);
```

先创建各节点，并赋值

```c++
n0->next=n1;
n1->next=n2;
n2->next=n3;
n3->next=n4;
```
再将各节点按顺序连接起来，即可初始化链表！

#### 插入节点
在链表中插入节点非常容易。假设我们想在相邻的两个节点 n0 和 n1 之间插入一个新节点 P ，则只需**改变两个节点指针**即可，时间复杂度为 O(1)。
```cpp
void insert(Node* n0,Node* p){
    Node* n1=n0->next;
    p->next=n1;
    n0->next=p;
}
```

```cpp
void insert(Node* n0,Node* p){
   p->next=n0->next;
   n0->next=p;
}
```

#### 删除节点

```cpp
/* 删除链表的节点 n0 之后的首个节点 */
void remove(Node* n0){
    if(n0->next==nullptr){
        return ;
    }
    Node* p=n0->next;
    Node* n1=p->next;
    n0->next=n1;
    //释放内存
    delete p;
}
```
{%note danger%}
尽管在删除操作完成后节点 P 仍然指向 n1 ，但实际上遍历此链表已经无法访问到 P ，这意味着 P 已经不再属于该链表了。
{%endnote%}



#### 访问节点
在链表中**访问节点的效率较低**。我们可以在 O(1)时间下访问数组中的任意元素。链表则不然，程序需要**从头节点出发**，逐个向后遍历，直至找到目标节点，时间复杂度为 O(n)。
```cpp
/* 访问链表中索引为 index 的节点 */
void access(Node* head,int index){
    for(int i=0;i<index;i++){
        if(head==nullptr){
            return nullptr;
        }
        head=head->next;
        //通过索引值i进行遍历计数
        //head指针不断后移，进行迭代，直至索引值
    }
    return head;
}
```

#### 查找节点
{% fold into @ Wrong Version %}
:x::x::x:
```cpp
//遍历链表，查找其中值为 target 的节点，输出该节点在链表中的索引
int find(Node* head,int target){
    int i=0;
    while(head!=nullptr&&head->val!=target){
        head=head->next;
        i++;
    }
    return i;
}
```
:x::x::x:
未检验是否找到目标节点

{% endfold %}
```cpp
//遍历链表，查找其中值为 target 的节点，输出该节点在链表中的索引
int find (Node* head,int target){
    int index=0;
    while(head!=nullptr){
        if(head->val==target){
            return index;
        }
        head=head->next;
        index++;
    }
    return -1;
}
```

## Array  VS.  Linkedlist

||数组|链表|
|:---:|:---:|:---:|
|存储方式|连续内存空间|分散内存空间|
|容量扩展|长度不可变|可灵活扩展|
|内存效率|元素占用内存少、但可能浪费空间|元素占用内存多|
|访问元素| O(1) | O(n) |
|添加元素| O(n) | O(1) |
|删除元素| O(n) | O(1) |


## 常见链表类型
![常见链表种类](https://github.com/Richard110206/Blog-image/blob/main/article/DataStructure/kindoflinkedlist.png?raw=true)

- **单向链表**：即前面介绍的普通链表。单向链表的节点包含值和指向下一节点的引用两项数据。我们将首个节点称为头节点，将最后一个节点称为尾节点，尾节点指向空 None 。

- **环形链表**：如果我们令**单向链表的尾节点指向头节点**（首尾相接），则得到一个环形链表。在环形链表中，任意节点都可以视作头节点。
```cpp
struct Node{
    int val;
    Node* next;
    Node(x):val(x),next(nullptr){}
};
Node* head=new Node(1);

   ...
   ...
   ...

   Node* tail=new Node(5);
   tail->next=head;
```

- **双向链表**：与单向链表相比，双向链表**记录了两个方向的引用**。双向链表的节点定义同时包含指向**后继节点**和**前驱节点**指针。相较于单向链表，双向链表更具**灵活性**，可以**朝两个方向遍历链表**，但相应地也需要占用更多的内存空间。


```cpp
struct Node{
    int val;
    Node* next;
    Node* prev;
    Node(x): val(x),next(nullptr),prev(nullptr){}
};
```

## 计算机存储设备

||硬盘|内存|缓存|
|:---:|:---:|:---:|:---:|
|用途|长期存储数据，包括操作系统、程序、文件等|临时存储当前运行的程序和正在处理的数据|存储经常访问的数据和指令，减少 CPU 访问内存的次数|
|易失性|断电后数据不会丢失|断电后数据会丢失|断电后数据会丢失|
|容量|较大，TB 级别|较小，GB 级别|非常小，MB 级别|
|速度|较慢，几百到几千 MB/s|较快，几十 GB/s|非常快，几十到几百 GB/s|
|价格（人民币）|较便宜，几毛到几元 / GB|较贵，几十到几百元 / GB|非常贵，随 CPU 打包计价|

- **硬盘**用于**长期存储大量数据**
- **内存**用于**临时存储程序运行中正在处理的数据**
- **缓存**用于**存储经常访问的数据和指令**


## Conclusion
1. **数组要求相同类型的元素，而在链表中不强调相同类型！**
- 链表由节点组成，节点之间通过指针连接，各个节点可以**存储不同类型的数据**，例如 int、double、string、object 等。
- 相对地，数组元素则必须是相同类型的，这样才能**通过计算偏移量来获取对应元素位置**。例如，数组同时包含 int 和 long 两种类型，单个元素分别占用 4 字节和 8 字节 。