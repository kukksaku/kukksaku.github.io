# 剑指offer之二维数组中的查找

### 题目描述：

在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

### 思路：

矩阵是有序的，从左下角来看，向上数字递减，向右数字递增，
因此从左下角开始查找，当要查找数字比左下角数字大时。右移
要查找数字比左下角数字小时，上移（同理从右上角也可以）
**思路**：从左下角开始遍历，左下角开始，遇大右移，遇小上移，直到超过边界都没找到，得false。否则得true。

### 代码实现：

#### python

```python
class Solution:
    def Find(self,target,array):
        row=len(array)
        col=len(array[0])
        if col==0:
            return False
        for i in range(row-1,-1.-1):
            if target>array[i][0]:
                for j in range(col):
                    if target==array[i][j]:
                        return Ture
                elif target==array[i][0]:
                    return Ture
        return False
```



#### C++

```C++
class Solution {
public:
    bool Find(int target, vector<vector<int> > array) {
        //形参vector<vector<int> >array表达的是array是一个二维的int型的vector（实际上相当于一个二维数组）,并且其实质是类
        //注意C++中只有关键字true、false而不是大写
        int row=array.size();
        int col=array[0].size();
        int i=row-1;
        int j=0;
        while（i>=0&&j<col){
            if(target>array[i][j]){
                j++;
                continue;
            }
            else if(target <array[i][j]){
                i--;
                continue;
            }
            else{
                return true;
            }
        }
        return false;
    }
};
```

##### 关于vector

vector(向量)是一种对象实体，属于STL（标准模板库）的一种自定义的数据类型, 可以广义上认为是数组的增强版。

 在使用它时, 需要包含头文件 vector, **#include<vector>**

vector 容器与数组相比其优点在于它能够根据需要随时自动调整自身的大小以便容下所要放入的元素。此外, vector 也提供了许多的方法来对自身进行操作。

vector< vector< int>>：
标准库模型vector表示对象的集合，其中所有对象的类型都相同。集合中每个对象都有一个与之对应索引，索引用于访问对象。

[向量的基本使用](https://www.cnblogs.com/mr-wid/archive/2013/01/22/2871105.html)

****

两种方法对vector<vecter<int>>进行赋值：

```C++
//法一：采用vector模板中的方法push_back()
#include<iostream>
#include<vector> 
using namespace std;

int main()
{
    //array用来保存一个3*3的二维数组，array的每个元素都是vector<int>类型
    vector <vector<int> >array;
    std::vector<int> v;
    for (int i = 0; i <3; i++){
        for (int j = 0; j <3; j++){
            int value;
            cin >> value;
            v.push_back(value);
        }
        array.push_back(v); //保存array的每个元素
        v.clear();
    }

    for (int i = 0; i <array.size(); i++)
    {
        for (int j = 0; j <3; j++)
            cout <<array[i][j];
        cout<<endl;
    }
    return 0;
}

//法二：用分配空间的resize()函数

#include<iostream>
#include<vector> 
using namespace std;

int main()
{
    vector <vector<int> >array(3);//首先给array开辟了三个空间
    for (int i = 0; i <3; i++){
        array[i].resize(3);//给array中每个元素开辟了三个空间
        for (int j = 0; j <3; j++){
            cin >> array[i][j];//直接对开辟的空间赋值即可
        }
    }
    for (int i = 0; i <array.size(); i++)
    {
        for (int j = 0; j <3; j++)
            cout <<array[i][j];
        cout<<endl;
    }
    cout << array.size();
    return 0;
}

```

