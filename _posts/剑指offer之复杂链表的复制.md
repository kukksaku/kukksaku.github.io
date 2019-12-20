# 剑指offer之复杂链表的复制

### 题目描述

输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

### C++

```C++
/*
struct RandomListNode {
    int label;
    struct RandomListNode *next, *random;
    RandomListNode(int x) :
            label(x), next(NULL), random(NULL) {
    }
};
*/
class Solution {
public:
    RandomListNode* Clone(RandomListNode* pHead)
    {
        while(pHead==NULL){
            return NULL;
        }
        RandomListNode* pNow=pHead;
        //复制原有结点及next
        while(pHead->next){
           RandomListNode* node=new RandomLisgtNode(pNow->label);//边复制边分配空间
            node->next=pNow->next;
            pNow->next=node;
            pNow=node->next;//移位，继续复制                    
            
        }
        pNow=pHead;
        //复制random，pNow是原有结点，pNow->next是复制的结点
        while(pNow！=NULL){
            if(pNow->random!=NULL){
                pNow->next->random=pNow->random->next;
            }
            pNow=pNow->next->next;
        }
        RandomListNode* head = pHead->next;
        RandomListNode* cur = head;
        pNow = pHead;
        //拆分链表
        while(pNow!=NULL){
            pNow->next = pNow->next->next;
            if(cur->next!=NULL){
                cur->next = cur->next->next;
            }
            cur = cur->next;
            pNow = pNow->next;
        }
        return head;
    }
};
```

