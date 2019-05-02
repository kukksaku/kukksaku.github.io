

---
layout:     post
title:      剑指offer之旋转数组的最小数字
subtitle:   算法、剑指offer、二分查找
date:       2019-5-2
author:     kkkkuboy
header-img: img/post-bg-ios9-web.jpg
catalog: 	 true
tags:
    - 剑指offer
    - 算法
 
    
---

# 剑指offer之旋转数组的最小数字

### 题目描述

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

### 解题思路

从题目得出信息：
输入：一个递增排序的数组的旋转数组
输出：该数组的最小值
思路：

1、最直接的方法，但是比较笨
先遍历，边遍历边比较，直到找到一个后一位的值小于前一位，则后一位为最小值。

2、二分查找
利用二分查找缩小范围即可：

mid = low + (high - low)/2 

  需要考虑三种情况： 

  (1)array[mid] > array[high]: 

  出现这种情况的array类似[3,4,5,6,0,1,2]，此时最小数字一定在mid的右边。 

  low = mid + 1 

  (2)array[mid] == array[high]: 

  出现这种情况的array类似 [1,0,1,1,1]   或者[1,1,1,0,1]，此时最小数字不好判断在mid左边 

  还是右边,这时只好一个一个试 ， 

  high = high - 1 

  (3)array[mid] < array[high]: 

  出现这种情况的array类似[2,2,3,4,5,6,6],此时最小数字一定就是array[mid]或者在mid的左 

  边。因为右边必然都是递增的。 

  high = mid 

  **注意这里有个坑：如果待查询的范围最后只剩两个数，那么mid** **一定会指向下标靠前的数字**

### 代码实现

##### C++

```C++
//法一：直接
class Solution {
public:
    int minNumberInRotateArray(vector<int> rotateArray) {
        int len=rotateArray.size();
        if(len==0){
            return 0;
        }
        int i=0;
        for(i=0;i<len;i++){
            if(rotateArray[i]>rotateArray[i+1]){
                return rotateArray[i+1];
            }
        }
        return rotateArray[0];
    }
};

//法二：二分查找
链接：https://www.nowcoder.com/questionTerminal/9f3231a991af4f55b95579b44b7a01ba
来源：牛客网

class Solution {
public:
    int minNumberInRotateArray(vector<int> rotateArray) {
        if (rotateArray.empty()) return 0;
        int left = 0, right = rotateArray.size() - 1;
        while (left < right) {
            //确认子数组是否是类似1,1,2,4,5,..,7的非递减数组
            if (rotateArray[left] < rotateArray[right]) return rotateArray[left];
             
            int mid = left + (right - left) / 2;
            //如果左半数组为有序数组
            if (rotateArray[left] < rotateArray[mid])
                left = mid + 1;
            //如果右半数组为有序数组
            else if (rotateArray[mid] < rotateArray[right])
                right = mid;
            //否则，rotateArray[left] == rotateArray[mid] == rotateArray[right]
            else {
                ++left;
            }
        }
        return rotateArray[left];
    }
};
```



##### python

```python
#方法一：直接min函数
class Solution:
    def minNumberInRotateArray(self, rotateArray):
        # write code here
        if not rotateArray:
            return 0
        else:
            return min(rotateArray)
        
        
 #方法二：先sort排序
class Solution:
    def minNumberInRotateArray(self, rotateArray):
        # write code here
        if not rotateArray:
            return 0
        else:
            rotateArray.sort()
            return rotateArray[0]

        
        
# 方法三：二分查找
class Solution:
    def minNumberInRotateArray(self, rotateArray):
        # write code here
        length = len(rotateArray)
        if length == 0:
           return 0
        elif length == 1:
            return rotateArray[0]
        else:
            left = 0
            right = length - 1
            while left < right:
                mid = (left + right)/2
                if rotateArray[mid] < rotateArray[j]:
                    right = mid
                else:
                    left = mid+1
            return rotateArray[i]
        
 #方法四：比较
class Solution:
    def minNumberInRotateArray(self, rotateArray):
        # write code here
        length = len(rotateArray)
        if length == 0:
           return 0
        elif length == 1:
            return rotateArray[0]
        else:
            for i in range(length-1):
                if rotateArray[i] > rotateArray[i+1]:
                    return rotateArray[i+1]
            return rotateArray[length-1]
        

```

