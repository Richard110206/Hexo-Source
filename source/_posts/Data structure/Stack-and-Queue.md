---
title: Stack and Queue
date: 2025-08-23 14:16:21
tags: [datastructure,stack,queue,updating]
index_img: https://github.com/Richard110206/Blog-image/blob/main/cover/Stack-and-Queue.png?raw=true
category: Data Structure
category_bar: true
description: Reviewing the chapter about stack and queue in Data Structure
---


## Stack
栈（stack）是一种遵循**先入后出**(**LIFO**)逻辑的线性数据结构:我们把堆叠元素的顶部称为“**栈顶**”，底部称为“**栈底**”。将把**元素添加到栈顶**的操作叫作“**入栈**”，**删除栈顶元素**的操作叫作“**出栈**”。

### Stack in STL
```cpp
#include <stack>
/* 初始化栈 */
stack<int> stack;

/* 元素入栈 */
stack.push(1);
stack.push(3);
stack.push(2);
stack.push(5);
stack.push(4);

/* 访问栈顶元素 */
int top = stack.top();

/* 元素出栈 */
stack.pop(); // 无返回值

/* 获取栈的长度 */
int size = stack.size();

/* 判断是否为空 */
bool empty = stack.empty();
```

## 栈的实现

&emsp;&emsp;由于**数组**和**链表**都可以在任意位置添加和删除元素，因此栈可以视为一种**受限制的**数组或链表。换句话说，我们可以“屏蔽”数组或链表的部分无关操作，使其对外表现的逻辑符合栈的特性，下面我们分别**用数组和链表实现栈的功能**！

### 基于数组的实现
使用数组实现栈时，我们可以将**数组的尾部作为栈顶**。由于入栈的元素可能会源源不断地增加，因此我们可以使用**动态数组**，这样就无须自行处理**数组扩容问题**。
```cpp
vector <int> stack;
/* 获取栈的长度 */
int size() {
    return stack.size();
}
/* 判断栈是否为空 */
bool empty() {
    return stack.size() == 0;
}
/* 入栈 */
void push(int num){
    stack.push_back(num);
}
/* 出栈 */
void pop() {
    stack.pop_back();
}
/* 访问栈顶元素 */
int top() {
    if (isempty()) {
        throw out_of_range("栈为空");
    }
    return stack.back();
}
```


### 基于链表的实现

使用链表实现栈时，我们可以将链表的**头节点视为栈顶**，**尾节点视为栈底**。
- 对于入栈操作，我们只需将**元素插入链表头部**，这种节点插入方法被称为“**头插法**”。
- 对于出栈操作，只需**将头节点从链表中删除**即可。

```cpp
/* 基于链表实现的栈 */
class LinkedListStack {
  private:
    ListNode *stackTop; // 将头节点作为栈顶
    int stkSize;        // 栈的长度

  public:
    LinkedListStack() {
        stackTop = nullptr;
        stkSize = 0;
    }

    ~LinkedListStack() {
        // 遍历链表删除节点，释放内存
        freeMemoryLinkedList(stackTop);
    }

    /* 获取栈的长度 */
    int size() {
        return stkSize;
    }

    /* 判断栈是否为空 */
    bool isEmpty() {
        return size() == 0;
    }

    /* 入栈 */
    void push(int num) {
        ListNode *node = new ListNode(num);
        node->next = stackTop;
        stackTop = node;
        stkSize++;
    }

    /* 出栈 */
    int pop() {
        int num = top();
        ListNode *tmp = stackTop;
        stackTop = stackTop->next;
        // 释放内存
        delete tmp;
        stkSize--;
        return num;
    }

    /* 访问栈顶元素 */
    int top() {
        if (isEmpty())
            throw out_of_range("栈为空");
        return stackTop->val;
    }

    /* 将 List 转化为 Array 并返回 */
    vector<int> toVector() {
        ListNode *node = stackTop;
        vector<int> res(size());
        for (int i = res.size() - 1; i >= 0; i--) {
            res[i] = node->val;
            node = node->next;
        }
        return res;
    }
};
```

## Queue
队列（queue）是一种遵循**先入先出**（**FIFO**)规则的线性数据结构。队列模拟了排队现象，即新来的人不断加入队列尾部，而位于队列头部的人逐个离开。我们将队列头部称为“**队首**”，尾部称为“**队尾**”，将把**元素加入队尾**的操作称为“**入队**”，**删除队首元素**的操作称为“**出队**”。

### Queue in STL
```cpp
#include <queue>
/* 初始化队列 */
queue<int> queue;

/* 元素入队 */
queue.push(1);
queue.push(3);
queue.push(2);
queue.push(5);
queue.push(4);

/* 访问队首元素 */
int front = queue.front();

/* 元素出队 */
queue.pop();

/* 获取队列的长度 */
int size = queue.size();

/* 判断队列是否为空 */
bool empty = queue.empty();
```

## 队列的实现
### 基于数组的实现
在数组中删除首元素的时间复杂度为 O(n) ，这会导致出队操作效率较低。然而我们可以使用一个变量 `front` 指向**队首元素的索引**，并维护一个变量 `size` 用于**记录队列长度**。定义 `rear = front + size` ，这个公式计算出的 `rear` 指向**队尾元素之后的下一个位置**。

基于此设计，数组中包含元素的**有效区间**为 [`front`, `rear` - 1]。

- **入队**操作：将输入元素赋值给 `rear` 索引处，并将 `size` 增加 1 。
- **出队**操作：只需将 `front` 增加 1 ，并将 `size` 减少 1 。


可以看到，入队和出队操作都只需进行一次操作，时间复杂度均为 O(1)

```cpp
/* 基于环形数组实现的队列 */
class ArrayQueue {
  private:
    int *nums;       // 用于存储队列元素的数组
    int front;       // 队首指针，指向队首元素
    int queSize;     // 队列长度
    int queCapacity; // 队列容量

  public:
    ArrayQueue(int capacity) {
        // 初始化数组
        nums = new int[capacity];
        queCapacity = capacity;
        front = queSize = 0;
    }

    ~ArrayQueue() {
        delete[] nums;
    }

    /* 获取队列的容量 */
    int capacity() {
        return queCapacity;
    }

    /* 获取队列的长度 */
    int size() {
        return queSize;
    }

    /* 判断队列是否为空 */
    bool isEmpty() {
        return size() == 0;
    }

    /* 入队 */
    void push(int num) {
        if (queSize == queCapacity) {
            cout << "队列已满" << endl;
            return;
        }
        // 计算队尾指针，指向队尾索引 + 1
        // 通过取余操作实现 rear 越过数组尾部后回到头部
        int rear = (front + queSize) % queCapacity;
        // 将 num 添加至队尾
        nums[rear] = num;
        queSize++;
    }

    /* 出队 */
    int pop() {
        int num = peek();
        // 队首指针向后移动一位，若越过尾部，则返回到数组头部
        front = (front + 1) % queCapacity;
        queSize--;
        return num;
    }

    /* 访问队首元素 */
    int peek() {
        if (isEmpty())
            throw out_of_range("队列为空");
        return nums[front];
    }

    /* 将数组转化为 Vector 并返回 */
    vector<int> toVector() {
        // 仅转换有效长度范围内的列表元素
        vector<int> arr(queSize);
        for (int i = 0, j = front; i < queSize; i++, j++) {
            arr[i] = nums[j % queCapacity];
        }
        return arr;
    }
};
```

### 基于链表的实现
```cpp
/* 基于链表实现的队列 */
class LinkedListQueue {
  private:
    ListNode *front, *rear; // 头节点 front ，尾节点 rear
    int queSize;

  public:
    LinkedListQueue() {
        front = nullptr;
        rear = nullptr;
        queSize = 0;
    }

    ~LinkedListQueue() {
        // 遍历链表删除节点，释放内存
        freeMemoryLinkedList(front);
    }

    /* 获取队列的长度 */
    int size() {
        return queSize;
    }

    /* 判断队列是否为空 */
    bool isEmpty() {
        return queSize == 0;
    }

    /* 入队 */
    void push(int num) {
        // 在尾节点后添加 num
        ListNode *node = new ListNode(num);
        // 如果队列为空，则令头、尾节点都指向该节点
        if (front == nullptr) {
            front = node;
            rear = node;
        }
        // 如果队列不为空，则将该节点添加到尾节点后
        else {
            rear->next = node;
            rear = node;
        }
        queSize++;
    }

    /* 出队 */
    int pop() {
        int num = peek();
        // 删除头节点
        ListNode *tmp = front;
        front = front->next;
        // 释放内存
        delete tmp;
        queSize--;
        return num;
    }

    /* 访问队首元素 */
    int peek() {
        if (size() == 0)
            throw out_of_range("队列为空");
        return front->val;
    }

    /* 将链表转化为 Vector 并返回 */
    vector<int> toVector() {
        ListNode *node = front;
        vector<int> res(size());
        for (int i = 0; i < res.size(); i++) {
            res[i] = node->val;
            node = node->next;
        }
        return res;
    }
};
```
{%fold into @ 在不断进行入队和出队的过程中，front 和 rear 都在向右移动，当它们到达数组尾部时就无法继续移动了，这时应该怎么办呢？%}

我们可以将数组视为首尾相接的“环形数组”。对于环形数组，我们需要让 `front` 或 `rear` 在越过数组尾部时，直接**回到数组头部继续遍历**。这种周期性规律可以通过“**取余操作**”来实现！

{%endfold%}

```cpp
/* 基于环形数组实现的队列 */
class ArrayQueue {
  private:
    int *nums;       // 用于存储队列元素的数组
    int front;       // 队首指针，指向队首元素
    int queSize;     // 队列长度
    int queCapacity; // 队列容量

  public:
    ArrayQueue(int capacity) {
        // 初始化数组
        nums = new int[capacity];
        queCapacity = capacity;
        front = queSize = 0;
    }

    ~ArrayQueue() {
        delete[] nums;
    }

    /* 获取队列的容量 */
    int capacity() {
        return queCapacity;
    }

    /* 获取队列的长度 */
    int size() {
        return queSize;
    }

    /* 判断队列是否为空 */
    bool isEmpty() {
        return size() == 0;
    }

    /* 入队 */
    void push(int num) {
        if (queSize == queCapacity) {
            cout << "队列已满" << endl;
            return;
        }
        // 计算队尾指针，指向队尾索引 + 1
        // 通过取余操作实现 rear 越过数组尾部后回到头部
        int rear = (front + queSize) % queCapacity;
        // 将 num 添加至队尾
        nums[rear] = num;
        queSize++;
    }

    /* 出队 */
    int pop() {
        int num = peek();
        // 队首指针向后移动一位，若越过尾部，则返回到数组头部
        front = (front + 1) % queCapacity;
        queSize--;
        return num;
    }

    /* 访问队首元素 */
    int peek() {
        if (isEmpty())
            throw out_of_range("队列为空");
        return nums[front];
    }

    /* 将数组转化为 Vector 并返回 */
    vector<int> toVector() {
        // 仅转换有效长度范围内的列表元素
        vector<int> arr(queSize);
        for (int i = 0, j = front; i < queSize; i++, j++) {
            arr[i] = nums[j % queCapacity];
        }
        return arr;
    }
};
```

# 双向队列

在队列中，我们仅能删除头部元素或在尾部添加元素。双向队列（double-ended queue）提供了**更高的灵活性**，允许在**头部和尾部**执行元素的添加或删除操作。

```cpp
#include <deque>
/* 初始化双向队列 */
deque<int> deque;

/* 元素入队 */
deque.push_back(2);   // 添加至队尾
deque.push_back(5);
deque.push_back(4);

deque.push_front(3);  // 添加至队首
deque.push_front(1);

/* 访问元素 */
int front = deque.front(); // 队首元素
int back = deque.back();   // 队尾元素

/* 元素出队 */
deque.pop_front();  // 队首元素出队
deque.pop_back();   // 队尾元素出队

/* 获取双向队列的长度 */
int size = deque.size();

/* 判断双向队列是否为空 */
bool empty = deque.empty();
```
## 双向队列的实现
### 基于数组的实现
```cpp
/* 基于环形数组实现的双向队列 */
class ArrayDeque {
  private:
    vector<int> nums; // 用于存储双向队列元素的数组
    int front;        // 队首指针，指向队首元素
    int queSize;      // 双向队列长度

  public:
    /* 构造方法 */
    ArrayDeque(int capacity) {
        nums.resize(capacity);
        front = queSize = 0;
    }

    /* 获取双向队列的容量 */
    int capacity() {
        return nums.size();
    }

    /* 获取双向队列的长度 */
    int size() {
        return queSize;
    }

    /* 判断双向队列是否为空 */
    bool isEmpty() {
        return queSize == 0;
    }

    /* 计算环形数组索引 */
    int index(int i) {
        // 通过取余操作实现数组首尾相连
        // 当 i 越过数组尾部后，回到头部
        // 当 i 越过数组头部后，回到尾部
        return (i + capacity()) % capacity();
    }

    /* 队首入队 */
    void pushFirst(int num) {
        if (queSize == capacity()) {
            cout << "双向队列已满" << endl;
            return;
        }
        // 队首指针向左移动一位
        // 通过取余操作实现 front 越过数组头部后回到尾部
        front = index(front - 1);
        // 将 num 添加至队首
        nums[front] = num;
        queSize++;
    }

    /* 队尾入队 */
    void pushLast(int num) {
        if (queSize == capacity()) {
            cout << "双向队列已满" << endl;
            return;
        }
        // 计算队尾指针，指向队尾索引 + 1
        int rear = index(front + queSize);
        // 将 num 添加至队尾
        nums[rear] = num;
        queSize++;
    }

    /* 队首出队 */
    int popFirst() {
        int num = peekFirst();
        // 队首指针向后移动一位
        front = index(front + 1);
        queSize--;
        return num;
    }

    /* 队尾出队 */
    int popLast() {
        int num = peekLast();
        queSize--;
        return num;
    }

    /* 访问队首元素 */
    int peekFirst() {
        if (isEmpty())
            throw out_of_range("双向队列为空");
        return nums[front];
    }

    /* 访问队尾元素 */
    int peekLast() {
        if (isEmpty())
            throw out_of_range("双向队列为空");
        // 计算尾元素索引
        int last = index(front + queSize - 1);
        return nums[last];
    }

    /* 返回数组用于打印 */
    vector<int> toVector() {
        // 仅转换有效长度范围内的列表元素
        vector<int> res(queSize);
        for (int i = 0, j = front; i < queSize; i++, j++) {
            res[i] = nums[index(j)];
        }
        return res;
    }
};
```

### 基于链表的实现

对于双向队列而言，头部和尾部都可以执行入队和出队操作。换句话说，双向队列需要实现另一个**对称方向**的操作。为此，我们采用“**双向链表**”作为双向队列的底层数据结构。

```cpp
/* 双向链表节点 */
struct DoublyListNode {
    int val;              // 节点值
    DoublyListNode *next; // 后继节点指针
    DoublyListNode *prev; // 前驱节点指针
    DoublyListNode(int val) : val(val), prev(nullptr), next(nullptr) {
    }
};

/* 基于双向链表实现的双向队列 */
class LinkedListDeque {
  private:
    DoublyListNode *front, *rear; // 头节点 front ，尾节点 rear
    int queSize = 0;              // 双向队列的长度

  public:
    /* 构造方法 */
    LinkedListDeque() : front(nullptr), rear(nullptr) {
    }

    /* 析构方法 */
    ~LinkedListDeque() {
        // 遍历链表删除节点，释放内存
        DoublyListNode *pre, *cur = front;
        while (cur != nullptr) {
            pre = cur;
            cur = cur->next;
            delete pre;
        }
    }

    /* 获取双向队列的长度 */
    int size() {
        return queSize;
    }

    /* 判断双向队列是否为空 */
    bool isEmpty() {
        return size() == 0;
    }

    /* 入队操作 */
    void push(int num, bool isFront) {
        DoublyListNode *node = new DoublyListNode(num);
        // 若链表为空，则令 front 和 rear 都指向 node
        if (isEmpty())
            front = rear = node;
        // 队首入队操作
        else if (isFront) {
            // 将 node 添加至链表头部
            front->prev = node;
            node->next = front;
            front = node; // 更新头节点
        // 队尾入队操作
        } else {
            // 将 node 添加至链表尾部
            rear->next = node;
            node->prev = rear;
            rear = node; // 更新尾节点
        }
        queSize++; // 更新队列长度
    }

    /* 队首入队 */
    void pushFirst(int num) {
        push(num, true);
    }

    /* 队尾入队 */
    void pushLast(int num) {
        push(num, false);
    }

    /* 出队操作 */
    int pop(bool isFront) {
        if (isEmpty())
            throw out_of_range("队列为空");
        int val;
        // 队首出队操作
        if (isFront) {
            val = front->val; // 暂存头节点值
            // 删除头节点
            DoublyListNode *fNext = front->next;
            if (fNext != nullptr) {
                fNext->prev = nullptr;
                front->next = nullptr;
            }
            delete front;
            front = fNext; // 更新头节点
        // 队尾出队操作
        } else {
            val = rear->val; // 暂存尾节点值
            // 删除尾节点
            DoublyListNode *rPrev = rear->prev;
            if (rPrev != nullptr) {
                rPrev->next = nullptr;
                rear->prev = nullptr;
            }
            delete rear;
            rear = rPrev; // 更新尾节点
        }
        queSize--; // 更新队列长度
        return val;
    }

    /* 队首出队 */
    int popFirst() {
        return pop(true);
    }

    /* 队尾出队 */
    int popLast() {
        return pop(false);
    }

    /* 访问队首元素 */
    int peekFirst() {
        if (isEmpty())
            throw out_of_range("双向队列为空");
        return front->val;
    }

    /* 访问队尾元素 */
    int peekLast() {
        if (isEmpty())
            throw out_of_range("双向队列为空");
        return rear->val;
    }

    /* 返回数组用于打印 */
    vector<int> toVector() {
        DoublyListNode *node = front;
        vector<int> res(size());
        for (int i = 0; i < res.size(); i++) {
            res[i] = node->val;
            node = node->next;
        }
        return res;
    }
};
```

{%fold into @ 撤销（undo）和反撤销（redo）具体是如何实现的？%}

使用两个栈，栈 A 用于**撤销**，栈 B 用于**反撤销**。

每当用户执行一个操作，将这个操作压入栈 A ，并清空栈 B 。
当用户执行“撤销”时，从栈 A 中弹出最近的操作，并将其压入栈 B 。
当用户执行“反撤销”时，从栈 B 中弹出最近的操作，并将其压入栈 A 。

{%endfold%}