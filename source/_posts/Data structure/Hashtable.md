---
title: Hashtable
date: 2025-08-26 12:07:53
tags: [algorithm]
index_img: https://github.com/Richard110206/Blog-image/blob/main/cover/Hashtable.png?raw=true
category: Data Structure
category_bar: true
description: This article explores some of the most essential sorting algorithms in Datastructure!
math: true
---

## Hashtable

&emsp;&emsp;**哈希表**（hash table），又称**散列表**，它通过建立**键key**与**值value**之间的**映射**，实现高效的**元素查询**。具体而言，我们向哈希表中输入一个键 key,则可以在 $O(1)$ 时间内获取对应的值value 。

同时数组和链表也可以实现查询功能，元素查询效率对比如图表所示：

| 操作     | 数组    | 链表    | 哈希表 |
| :-------: | :------: | :------: | :-----: |
| 查找元素 | O(n) | O(n) | O(1) |
| 添加元素 | O(1) | O(1) | O(1) |
| 删除元素 | O(n) | O(n) | O(1) |

可以发现，在哈希表中进行**增删查改**的时间复杂度都是 $O(1)$ ，非常高效。

### Hashtable in STL
[详情请看这篇博客Problem A的注释](https://richard110206.github.io/2025/06/27/Data%20structure/CUMT-Datastructure-Practice-4/)

### 哈希表的实现
&emsp;&emsp;我们先考虑最简单的情况，仅用一个数组来实现哈希表。在哈希表中，我们将**数组中的每个空位**称为**桶**（bucket），每个桶可**存储一个键值对**。因此，查询操作就是找到key对应的桶，并在桶中获取value。

&emsp;&emsp;这是通过**哈希函数**（hash function）实现的，它能将一个较大的输入空间**映射**到一个较小的输出空间。在哈希表中，输入空间是所有key，输出空间是所有桶（**数组索引**）。换句话说，输入一个key，我们可以通过哈希函数得到该key对应的键值对**在数组中的存储位置**。

输入一个key，哈希函数的计算过程分为以下两步。
1. 通过某种**哈希算法**hash()计算得到**哈希值**。
2. 将**哈希值对桶数量（数组长度）capacity 取模**，从而获取该key对应的数组索引index。

```
index = hash(key) % capacity
```

随后，我们就可以利用index在哈希表中访问对应的桶，从而获取value。

```cpp
/* 键值对 */
struct Pair {
  public:
    int key;
    string val;
    Pair(int key, string val) {
        this->key = key;
        this->val = val;
    }
};

/* 基于数组实现的哈希表 */
class ArrayHashMap {
  private:
    vector<Pair *> buckets;

  public:
    ArrayHashMap() {
        // 初始化数组，包含 100 个桶
        buckets = vector<Pair *>(100);
    }

    ~ArrayHashMap() {
        // 释放内存
        for (const auto &bucket : buckets) {
            delete bucket;
        }
        buckets.clear();
    }

    /* 哈希函数 */
    int hashFunc(int key) {
        int index = key % 100;
        return index;
    }

    /* 查询操作 */
    string get(int key) {
        int index = hashFunc(key);
        Pair *pair = buckets[index];
        if (pair == nullptr)
            return "";
        return pair->val;
    }

    /* 添加操作 */
    void put(int key, string val) {
        Pair *pair = new Pair(key, val);
        int index = hashFunc(key);
        buckets[index] = pair;
    }

    /* 删除操作 */
    void remove(int key) {
        int index = hashFunc(key);
        // 释放内存并置为 nullptr
        delete buckets[index];
        buckets[index] = nullptr;
    }

    /* 获取所有键值对 */
    vector<Pair *> pairSet() {
        vector<Pair *> pairSet;
        for (Pair *pair : buckets) {
            if (pair != nullptr) {
                pairSet.push_back(pair);
            }
        }
        return pairSet;
    }

    /* 获取所有键 */
    vector<int> keySet() {
        vector<int> keySet;
        for (Pair *pair : buckets) {
            if (pair != nullptr) {
                keySet.push_back(pair->key);
            }
        }
        return keySet;
    }

    /* 获取所有值 */
    vector<string> valueSet() {
        vector<string> valueSet;
        for (Pair *pair : buckets) {
            if (pair != nullptr) {
                valueSet.push_back(pair->val);
            }
        }
        return valueSet;
    }

    /* 打印哈希表 */
    void print() {
        for (Pair *kv : pairSet()) {
            cout << kv->key << " -> " << kv->val << endl;
        }
    }
};
```


### 哈希冲突
&emsp;&emsp;从本质上看，哈希函数的作用是将所有 key 构成的**输入空间映射到数组所有索引构成的输出空间**，而输入空间往往远大于输出空间。因此，理论上一定存在“**多个输入对应相同输出**”的情况,我们将这种多个输入对应同一输出的情况称为**哈希冲突**（hash collision）。
&emsp;&emsp;哈希冲突会导致查询结果错误，严重影响哈希表的可用性。为了解决该问题，每当遇到哈希冲突时，我们就进行哈希表扩容，直至冲突消失为止。此方法简单粗暴且有效，但效率太低。为了提升效率，我们可以采用以下策略：

1. **改良哈希表数据结构**，使得哈希表可以在出现哈希冲突时正常工作。
- **“链式地址”**
- **“开放寻址”**
2. 仅在必要时，即当哈希冲突比较严重时，才执行扩容操作。

#### 链式地址
&emsp;&emsp;在原始哈希表中，每个桶仅能存储一个键值对。**链式地址**（separate chaining）将**单个元素转换为链表**，将键值对作为链表节点，将所有**发生冲突的键值对都存储在同一链表中**。

![链式地址哈希表](https://github.com/Richard110206/Blog-image/blob/main/article/DataStructure/linkedlisthash.png?raw=true)

&emsp;&emsp;基于链式地址实现的哈希表，需要通过哈希函数**访问链表头节点**，**遍历链表到目标节点**在进行增删改查的操作。

{%note info%}
**Limitations**

- **占用空间增大**：链表包含节点指针，它相比数组更加**耗费内存空间**。
- **查询效率降低**：因为需要**线性遍历链表**来查找对应元素。
{%endnote%}


```cpp
/* 链式地址哈希表 */
class HashMapChaining {
  private:
    int size;                       // 键值对数量
    int capacity;                   // 哈希表容量
    double loadThres;               // 触发扩容的负载因子阈值
    int extendRatio;                // 扩容倍数
    vector<vector<Pair *>> buckets; // 桶数组

  public:
    /* 构造方法 */
    HashMapChaining() : size(0), capacity(4), loadThres(2.0 / 3.0), extendRatio(2) {
        buckets.resize(capacity);
    }

    /* 析构方法 */
    ~HashMapChaining() {
        for (auto &bucket : buckets) {
            for (Pair *pair : bucket) {
                // 释放内存
                delete pair;
            }
        }
    }

    /* 哈希函数 */
    int hashFunc(int key) {
        return key % capacity;
    }

    /* 负载因子 */
    double loadFactor() {
        return (double)size / (double)capacity;
    }

    /* 查询操作 */
    string get(int key) {
        int index = hashFunc(key);
        // 遍历桶，若找到 key ，则返回对应 val
        for (Pair *pair : buckets[index]) {
            if (pair->key == key) {
                return pair->val;
            }
        }
        // 若未找到 key ，则返回空字符串
        return "";
    }

    /* 添加操作 */
    void put(int key, string val) {
        // 当负载因子超过阈值时，执行扩容
        if (loadFactor() > loadThres) {
            extend();
        }
        int index = hashFunc(key);
        // 遍历桶，若遇到指定 key ，则更新对应 val 并返回
        for (Pair *pair : buckets[index]) {
            if (pair->key == key) {
                pair->val = val;
                return;
            }
        }
        // 若无该 key ，则将键值对添加至尾部
        buckets[index].push_back(new Pair(key, val));
        size++;
    }

    /* 删除操作 */
    void remove(int key) {
        int index = hashFunc(key);
        auto &bucket = buckets[index];
        // 遍历桶，从中删除键值对
        for (int i = 0; i < bucket.size(); i++) {
            if (bucket[i]->key == key) {
                Pair *tmp = bucket[i];
                bucket.erase(bucket.begin() + i); // 从中删除键值对
                delete tmp;                       // 释放内存
                size--;
                return;
            }
        }
    }

    /* 扩容哈希表 */
    void extend() {
        // 暂存原哈希表
        vector<vector<Pair *>> bucketsTmp = buckets;
        // 初始化扩容后的新哈希表
        capacity *= extendRatio;
        buckets.clear();
        buckets.resize(capacity);
        size = 0;
        // 将键值对从原哈希表搬运至新哈希表
        for (auto &bucket : bucketsTmp) {
            for (Pair *pair : bucket) {
                put(pair->key, pair->val);
                // 释放内存
                delete pair;
            }
        }
    }

    /* 打印哈希表 */
    void print() {
        for (auto &bucket : buckets) {
            cout << "[";
            for (Pair *pair : bucket) {
                cout << pair->key << " -> " << pair->val << ", ";
            }
            cout << "]\n";
        }
    }
};
```


#### 开放寻址
开放寻址（open addressing）不引入额外的数据结构，而是通过“多次探测”来处理哈希冲突，探测方式主要包括**线性探测**、**平方探测**和**多次哈希**等。

1. **线性探测**

&emsp;&emsp;通过哈希函数计算桶索引，若发现桶内已有元素，则**从冲突位置向后线性遍历**（步长通常为1，**固定步长索引**），直至找到空桶（none）/目标元素。

&emsp;&emsp;然而，线性探测容易产生“**聚集现象**”。具体来说，**数组中连续被占用的位置越长**，这些连续位置**发生哈希冲突的可能性越大**，从而进一步促使该位置的聚堆生长，形成恶性循环，最终导致增删查改操作**效率劣化**。

***

2. **平方探测**
&emsp;&emsp;平方探测与线性探测类似，都是开放寻址的常见策略之一。当发生冲突时，平方探测**不是简单地跳过一个固定的步数**，而是跳过“**探测次数的平方**”的步数，即1、4、9......步。

{%note info%}
**Advantages**
- 平方探测通过跳过探测次数平方的距离，试图**缓解线性探测的聚集效应**。
- 平方探测会跳过更大的距离来寻找空位置，有助于**数据分布得更加均匀**。
{%endnote%}
{%note info%}
**Limitations**
- **仍然存在聚集现象**，即某些位置比其他位置更容易被占用。
- 由于平方的增长，平方探测可能不会探测整个哈希表，这意味着即使哈希表中有空桶，平方探测也可能**无法访问**到它。
{%endnote%}

***

3. **多次哈希**
顾名思义，多次哈希方法**使用多个哈希函数** $f(x),g(x),k(x)$ 进行探测。

***

{%note danger%}
:warning:注意：**开放寻址**哈希表都存在“**不能直接删除元素**”的问题。

&emsp;&emsp;这是因为删除元素会在数组内产生一个空桶`None`，而当查询元素时，线性探测到该空桶就会返回，因此在**该空桶之下的元素都无法再被访问到**，程序可能**误判这些元素不存在**。

&emsp;&emsp;为了解决该问题，我们可以采用**懒删除机制**：它不直接从哈希表中移除元素，而是利用一个常量`TOMBSTONE`来标记这个桶。在该机制下，`None`和`TOMBSTONE`都代表空桶，都可以放置键值对。但不同的是，线性探测到`TOMBSTONE`时应该**继续遍历**，因为其之下可能还存在键值对。

&emsp;&emsp;然而，懒删除可能会加速哈希表的性能退化。这是因为每次删除操作都会产生一个删除标记，随着`TOMBSTONE`的增加，搜索时间也会增加，因为线性探测可能需要跳过多个`TOMBSTONE`才能找到目标元素。

&emsp;&emsp;为此，考虑在线性探测中记录遇到的首个`TOMBSTONE`的索引，并将搜索到的目标元素与该`TOMBSTONE`交换位置。这样做的好处是当每次查询或添加元素时，元素会被移动至距离理想位置（探测起始点）更近的桶，从而**优化查询效率**。
{%endnote%}
***

### 哈希扩容
&emsp;&emsp;哈希表容量越大，多个 key 被分配到同一个桶中的概率就越低，冲突就越少。因此，我们可以通过扩容哈希表来减少哈希冲突。

- **类似于数组扩容**，哈希表扩容需将所有键值对从原哈希表迁移至新哈希表，非常耗时
- 哈希表容量 capacity 改变，我们需要通过哈希函数来**重新计算所有键值对的存储位置**，这进一步增加了扩容过程的计算开销
  
为此，编程语言通常会预留足够大的哈希表容量，防止频繁扩容。

&emsp;&emsp;**负载因子**（load factor）是哈希表的一个重要概念，其定义为**哈希表的元素数量除以桶数量**，用于**衡量哈希冲突的严重程度**，也常作为哈希表扩容的触发条件。例如，当负载因子超过 0.75 时，我们可以考虑扩容哈希表。

### 哈希算法

&emsp;&emsp;前面的方法只能处理哈希冲突，并不能从**本质上**减少哈希冲突。如果哈希冲突**过于频繁**，哈希表的性能则会急剧劣化。对于链式地址哈希表，理想情况下**键值对均匀分布**在各个桶中，达到最佳查询效率；最差情况下所有键值对**都存储到同一个桶中**，时间复杂度退化至$O(n)$。

![链式存储最好最坏情况](https://github.com/Richard110206/Blog-image/blob/main/article/DataStructure/condition.png?raw=true)

我们制定出的哈希算法应达到以下目标：
- **确定性**：对于相同的输入，哈希算法应**始终产生相同的输出**。这样才能确保哈希表是可靠的。
- **效率高**：计算哈希值的过程应该**足够快**。**计算开销越小**，哈希表的**实用性越高**。
- **均匀分布**：哈希算法应使得**键值对均匀分布在哈希表**中。分布越均匀，哈希冲突的概率就越低。

{%fold into @为什么不使用哈希函数$f(x)=x$呢？这样就不会有冲突了！%}
在 $f(x)=x$ 哈希函数下，每个元素对应唯一的桶索引，这与数组等价。然而给定的数据空间是未知的，如果数据范围较为稀疏如学号12592、16754，那么数组的绝大部分空间都将被闲置，导致空间复杂度急剧上升。并且我们的目标是在有限的数组范围内存储较大数量的键值，也就是将一个较大的状态空间映射到一个较小的空间。
{%endfold%}