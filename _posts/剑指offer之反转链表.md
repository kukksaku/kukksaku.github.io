# 剑指offer之反转链表

### 题目描述

输入一个链表，反转链表后，输出新链表的表头。

**思路：使用两个指针，分别指向相邻结点，使后一个结点next指向前一个结点，直至遍历整个链表**



### C++

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
    ListNode* ReverseList(ListNode* pHead) {
        if(pHead==nullptr){
            return nullptr;
        }
        if(pHead->next==nunllptr){
            return pHead;
        }
        ListNode* p=pHead->next;
        ListNode* q=NULL;
        ListNode* h=pHead;
        while(p!=nullptr){
            h->next=q;
            q=h;
            h=p
            p=p>next;
        }
        h>next=q;
        pHead=h
        return pHead;
    }
};
```

