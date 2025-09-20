---
title: BinaryTree
date: 2025-09-17 12:57:11
tags: [Data Structure,BinaryTree]
index_img: https://github.com/Richard110206/Blog-image/blob/main/cover/BinaryTree.png?raw=true
math: true
category: Data Structure
category_bar: true
description: Covers binary tree traversals and introduces AVL trees as a self-balancing structure. Explains the core left and right rotation operations used to maintain balance.
---

## 二叉树
二叉树（binary tree）是一种非线性数据结构，代表“祖先”与“后代”之间的**派生关系**，体现了“一分为二”的分治逻辑。与链表类似，二叉树的基本单元是节点，每个节点包含**值**、**左子节点引用**和**右子节点引用**。
```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x):val(x),left(nullptr),right(nullptr){}
};
```
### 二叉树基本操作
#### 初始化
```cpp
TreeNode n1=new TreeNode(1);
TreeNode n2=new TreeNode(2);
TreeNode n2=new TreeNode(3);
TreeNode n1=new TreeNode(4);
TreeNode n2=new TreeNode(5);

n1->left=n2;
n1->right=n3;
n2->left=n4;
n2->right-n5;
```
#### 插入与删除节点
```cpp
TreeNode p=new TreeNode(0);
n1->left=p;
p->left=n2;

n1->left=n2;
delete p;
```

### 二叉树的基本类型

#### 完美二叉树
**所有层的节点都被完全填满**。在完美二叉树中，叶节点的度为0 ，其余所有节点的度都为2 ；若树的高度为$h$ ，则节点总数为$2^h-1$
 
#### 完全二叉树
仅允许**最底层的节点不完全填满**，且最底层的节点必须**从左至右依次连续填充**。

#### 平衡二叉树
节点的左子树和右子树的**高度之差的绝对值**不超过 1 。

#### 退化二叉树
所有节点都偏向一侧变成链表。

### 二叉树的遍历
次序遍历通常采用采用**深度优先遍历**（DFS），可以使用**递归**实现！
#### 前序遍历
```cpp
vector <int> preOrder; 
void preOrder(TreeNode* root){
    if(root==nullptr){
        return ;
    }
    preOrder.push_back(root->val);
    preOrder(root->left);
    preOrder(root->right);
}
```
#### 中序遍历
```cpp
vector <int> inOrder;
void inOrder(TreeNode* root){
    if(root==nullptr){
        return ;
    }
    inOrder(root->left);
    inOrder.push_back(root->val);
    inOrder(root->right);
}
```
#### 后续遍历
```cpp
vector <int> postOrder;
void postOrder(TreeNode* root){
    if(root==nullptr){
        return ;
    }
    postOrder(root->left);
    postOrder(root->right);
    postOrder.push_back(root->val);
}
```

#### 层序遍历
在**从顶部开始**向下，每一层**从左到右**的顺序访问节点，本质是**广度优先遍历**（BFS），可以使用**队列**来实现：
```cpp
vector<int> levelOrder(TreeNode* root){
    vector<int> result;
    if(root==nullptr){
        return result;
    }
    queue<TreeNode*> pq;
    pq.push(root);
    while(!pq.empty()){
        TreeNode* node=pq.front();
        pq.pop();
        result.push_back(node->val);
        if(node->left!=nullptr){
            pq.push(node->left);
        }
        if(node->right!=nullptr){
            pq.push(nopde->right);
        }
    }
return result;
}
```

### 数组创建二叉树
#### 完美二叉树
应用**映射公式**父节点和子节点的编号关系进行索引。若某节点的索引为$i$，则该节点的**左子节点**索引为$2i+1$，**右子节点**索引为$2i+2$ 
#### 任意二叉树
显式的写出所有`None`节点代表空节点，其中完全二叉树`None`均在末尾填充。

### 二叉搜索树
- 对于根节点，左子树中所有节点的值 $<$ 根节点的值 $<$ 右子树中所有节点的值。
- 任意节点的左、右子树也是二叉搜索树

其本质和二分查找算法一致，每次排除一半的情况，循环次数最多为二叉树的高度，当二叉树平衡时$O(logn)$
```cpp
TreenNode* search(TreeNode* root,int num){
    TreeNode* cur=root;
    while(cur!=nullptr){
        if(cur->val>num){
            cur=cur->left;
        }
        else if(cur->val<num){
            cur=cur->right;
        }
        else break;
    }
    return cur;
}
```

#### 插入节点
```cpp
TreeNode* insert(Treenode* root,int num){
    if(!root) return new TreeNode(num);
    TreeNode* cur=root,*pre=nullptr;
    while(cur!=nullptr){
        if(num==cur->val){
            return ;
        }
        pre=cur;
        else if(num>cur->val){
            cur=cur->right;
        }
        else cur=cur->left;
    }
    TreeNode* node=new TreeNode(num);
    if(num>pre->val) pre->right=node;
    else pre->left=node;
}
```
#### 删除节点
- 节点为叶子节点：直接删除
- 节点有一个度：删除该节点，并把该节点的**度节点替换成该节点**
- 节点有两个度：找到该节点的**右子树最小节点**，或者**左子树最大节点**，将这个节点替换成该节点，并删除该节点
```cpp
void deleteNode(TreeNode* root,int key){
    TreeNode* node = root;
    TreeNode* pre = nullptr;
    if(node==nullptr) return ;
    while(node!=nullptr){
    if(node->val==key) break;
    pre=node;
    else if(node->val>key) node=node->left;
    else node=node->right;
    }
    // 1.节点为叶子节点：
    if(!node->left&&!node->right){
        delete node;
    }
    // 2.节点有一个度
    // 2.1 节点只有左子树
    else if(node->left){
        pre->left==node?pre->left:pre->right=node->left;
        delete node;
    }
    // 2.2 节点只有右子树
    else if(node->right){
        pre->left==node?pre->left:pre->right=node->right;
        delete node;
    }
    // 3.节点有两个度
    else {
    TreeNode *tmp = node->right;
    while (tmp->left != nullptr) {
        tmp = tmp->left;
    }
    int tmpVal = tmp->val;
    deleteNode(tmp);
    node->val = tmpVal;
    }
}
```
## AVL 树
我们知道，二叉树在经过多次插入和删除后，可能会退化成链表，导致搜索效率下降。为了解决这个问题，提出了**AVL 树**，它是一种**自平衡二叉搜索树**（既是二叉搜索树，也是平衡二叉树，同时满足这两类二叉树的所有性质）。，任何节点的**左右子树高度差的绝对值不超过 1**。
### 节点高度

**从该节点到它的最远叶节点的距离**，即**所经过的“边”的数量**。

```cpp
struct TreeNode{
    int val;
    int height=0;
    TreeNode* left{};
    TreeNode* right{};
    TreeNode() = default;// 使用编译器生成的默认构造函数
    explicit TreeNode(int x) : val(x){} // 禁止隐式转换
}
```
```cpp
int height(TreeNode *node) {
    return node == nullptr ? -1 : node->height;
}

void updateHeight(TreeNode *node) {
    node->height = max(height(node->left), height(node->right)) + 1;
}
```

### 节点平衡因子
**为左子树的高度减去右子树的高度**
```cpp
int balanceFactor(TreeNode *node) {
    if (node == nullptr)
        return 0;
    return height(node->left) - height(node->right);
}
```
### 旋转
AVL树通过**旋转**，使得在不影响二叉树的**中序遍历序列**的前提下，使失衡节点重新恢复平衡。

#### 右旋
```cpp
TreeNode* rightRotate(TreeNode* node){
    TreeNode* child=node->left;
    TreeNode* grandChild=child->right;
    child->right=node;
    node->left=grandChild;
    updateHeight(node);
    updateHeight(child);
    return child;
}
```
#### 左旋
```cpp
TreeNode* leftRotate(TreeNode* node){
    TreeNode* child=node->right;
    TreeNode* grandChild=child->left;
    child->left=node;
    node->right=grandChild;
    updateHeight(node);
    updateHeight(child);
    return child;
}
```

#### 先左旋后右旋
```cpp
TreeNode* leftRightRotate(TreeNode* node){
    node->left=leftRotate(node->left);
    return rightRotate(node);
}
```
#### 先右旋后左旋
```cpp
TreeNode* rightLeftRotate(TreeNode* node){
    node->right=rightRotate(node->right);
    return leftRotate(node);
}
```
