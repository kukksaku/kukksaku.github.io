# 剑指offer之两个链表的第一个公共结点

### 题目描述

输入两个链表，找出它们的第一个公共结点。

**思路：**循环比较，直到第一个相等的结点

### 代码实现

#### C++

```C++
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) {
        while(pHead1==NULL||pHead2==NULL){
            return NULL;
        }
        ListNode* p1=pHead1;
        ListNode* p2=pHead2;
        while(p1->next!=NULL&&p2!=NULL){
            if(p1->val==p2->val&&p1->next->val==p2->next->val){
                break;
            }
            p1=p1->next;
            p2=p2->next;
        }
        if(p1->next==NULL||p2->next==NULL){
            return NULL;
        }
        return p1;
        
    }
};
//段错误:您的程序发生段错误，可能是数组越界，堆栈溢出（比如，递归调用层数太多）等情况引起
//但是在本地可以通过
//修改：
ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) {
        while(pHead1==NULL||pHead2==NULL){
            return NULL;
        }
        ListNode* p1=pHead1;
        ListNode* p2=pHead2;
        while(p1->next!=NULL&&p2!=NULL){
            if(p1->val==p2->val){
                if(p1->next!=NULL&&p2->next!=NULL&&p1->next->val==p2->next->val){
                    break;
                }
                if(p1->next==NULL&&p2->next==NULL){
                    return p1;
                }
            }
            p1=p1->next;
            p2=p2->next;
        }
        if(p1->next==NULL||p2->next==NULL){
            return NULL;
        }
        return p1;
        
    }
//我觉得可以但h是任然发生段错误

```

段错误常见情况：
1.vector为空，去访问a[i]，即vector中的某一个位置的值。

2.二叉树指针为NULL，却去访问左右节点，类似tree->left。所以在访问前的前提条件，一般要if二叉树的指针不为空a。

**网上思路及源码**

公共结点的样子：

[![剑指Offer（三十六）：两个链表的第一个公共结点](https://cuijiahua.com/wp-content/uploads/2018/01/basis_36_1.png)](https://cuijiahua.com/wp-content/uploads/2018/01/basis_36_1.png)

上图就是一个有公共结点的例子，在公共结点之后，两个链表指针指向的地址是相同的。

这道题有两个解法。

方法一：

我们可以把两个链表拼接起来，一个pHead1在前pHead2在后，一个pHead2在前pHead1在后。这样，生成了两个相同长度的链表，那么我们只要同时遍历这两个表，就一定能找到公共结点。时间复杂度O(m+n)，空间复杂度O(m+n)。

方法二：

我们也可以先让把长的链表的头砍掉，让两个链表长度相同，这样，同时遍历也能找到公共结点。此时，时间复杂度O(m+n)，空间复杂度为O(MAX(m,n))。

源码：

**C++**

```C++
/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
            val(x), next(NULL) {
    }
};*/
class Solution {
public:
    ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) {
        // 如果有一个链表为空，则返回结果为空
        if(pHead1 == NULL || pHead2 == NULL){
            return NULL;
        }
        // 获得两个链表的长度
        unsigned int len1 = GetListLength(pHead1);
        unsigned int len2 = GetListLength(pHead2);
        // 默认 pHead1 长， pHead2短，如果不是，再更改
        ListNode* pHeadLong = pHead1;
        ListNode* pHeadShort = pHead2;
        int LengthDif = len1 - len2;
        // 如果 pHead1 比 pHead2 小
        if(len1 < len2){
            ListNode* pHeadLong = pHead2;
            ListNode* pHeadShort = pHead1;
            int LengthDif = len2 - len1;
        }
        // 将长链表的前面部分去掉，使两个链表等长
        for(int i = 0; i < LengthDif; i++){
            pHeadLong = pHeadLong->next;
        }
        
        while(pHeadLong != NULL && pHeadShort != NULL && pHeadLong != pHeadShort){
            pHeadLong = pHeadLong->next;
            pHeadShort = pHeadShort->next;
        }
        return pHeadLong;
    }
private:
    // 获得链表长度
    unsigned int GetListLength(ListNode* pHead){
        if(pHead == NULL){
            return 0;
        }
        unsigned int length = 1;
        while(pHead->next != NULL){
            pHead = pHead->next;
            length++;
        }
        return length;
    }
};
```



**python：**

```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    def FindFirstCommonNode(self, pHead1, pHead2):
        # write code here
        if pHead1 == None or pHead2 == None:
            return None
        cur1, cur2 = pHead1, pHead2
        while cur1 != cur2:
            cur1 = cur1.next if cur1 != None else pHead2
            cur2 = cur2.next if cur2 != None else pHead1
        return cur1
```

