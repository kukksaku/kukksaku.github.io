# 剑指offer之从尾到头打印链表

## 题目描述

> 输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。
>
> **基本思路：将链表按顺序依次入栈，再将元素依次出栈放入一个容器中（倒序都可以想到栈）**

### 代码实现

### C++



```C++ 
/**
*  struct ListNode {
*        int val;
*        struct ListNode *next;
*        ListNode(int x) :
*              val(x), next(NULL) {
*        }
*  };
*/
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        ListNode *p=head;
        vector<int> value;
        stack<int> Nstack;
        while(p!=nullptr){
            Nstack.push(p->val);
            p=p->next;
        }
        int len=Nstack.size();
        while(!Nstack.empty()){
            value.push_back(Nstack.top());
            Nstack.pop();
        }
        return value;
    }
};
```



#### 关于C++STL中的堆栈容器stack