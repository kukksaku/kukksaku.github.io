# 剑指offer之链表中倒数第k个结点

## 题目描述

输入一个链表，输出该链表中倒数第k个结点。

**思路：**
法一：可以先把链表放到一个栈里，然后出栈，直到第k个（但是最好是其他类型的数）
法二：使用两个指针，相隔k-1个。到靠后的指针刚好指向空值时，前一个指针指向即为倒数第k个

（用实例分析）

### C++

需要注意代码的鲁棒性： 

- 链表为空；
- k == 0，以为k为无符号整数，k-1=0xFFFFFFFF，导致错误；
- 链表数不够k的情况，也就是第一个指针移动过程中变为nullptr。

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
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        ListNode *p=pListHead;
        stack<ListNode*> nodes;
        while(p!=nullptr){
            nodes.push(p);
            p=p->next;
        }
        unsigned int len=nodes.size();
        if(k>len){
           exit（1）;
        }
        else{
            for(unsigned int i=1;i<k;i++){
                nodes.pop();
            }
        }
        struct ListNode *q;
        q=new ListNode；
        
        if(!q){
            exit（1）;
        }
        q=nodes.top();
        return q;
    }
};
```

法二：

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
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        ListNode *p=pListHead;
        ListNode *q=pListHead;
        if(k<1||pListHead==nullptr){
            return NULL;
        }
        for(unsigned int i=1;i<k;i++){
            if(p!=nullptr){
                p=p->next;
            }
            else{
               return NULL; 
            }      
        }
         while(p->next！=nullptr){
                q=q->next;
                p=p->next;
            }                
       
        return q;
    }
};
//但是不知道为什么一直不通过

//去找了别人的代码如下
class Solution {
public:
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        if(pListHead == nullptr || k == 0)
            return nullptr;

        ListNode* pAhead = pListHead;
        ListNode* pBehind = nullptr;

        for(unsigned int i=0; i<k-1; i++){
            if(pAhead->next != nullptr)
                pAhead = pAhead->next;
            else
                return nullptr;
        }

        pBehind = pListHead;
        while(pAhead->next != nullptr){
            pAhead = pAhead->next;
            pBehind = pBehind->next;
        }

        return pBehind;
    }
};

```