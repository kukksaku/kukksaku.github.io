# 剑指offer之替换空格

### 题目描述

请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

### 代码实现

##### C++

```C++
class Solution {
public:
	void replaceSpace(char *str,int length) {
        int l=length;
        for(int i=0;i<l;i++){
            if(int(str[i])==32){
                str[i]="%20";
            }
        }
        

	}
};
/*编译错误:您提交的代码无法完成编译
In file included from a.cc:2:
./solution.h:7:23: error: assigning to 'char' from incompatible type 'const char [4]'
str[i]="%20";
^~~~~~
1 error generated.
*/
//傻*做法，首先没有注意这是一个字符转换为四个字符，要考虑字符串长度变化问题，还要考虑整体移位问题
//①若是从前往后遇到空格就转化，那么需要想就后面的每一个字符后移两个字节，这样时间复杂度就是O（n^2)
//②从后往前替换，先遍历一遍，计算出所有空格数，然后计算出最后字符串长度，然后从后往前（再循环内部就可以直接位移）转换

//修改
class Solution {
public:
	void replaceSpace(char *str,int length) {
       int l=length;
        int k=0;
        for(int i=0;i<l-1;i++){
            if(int(str[i])==32){
                k=k+2;
            }
        }
        for(int j=l;j>=0;j--){
            if(int(str[j])==32){
               str[j+k]='0';
               str[j+k-1]='2';
               str[j+k-2]='%';
                k=k-2;
            }
            else{str[j+k]=str[j];}
        }
                                 
	}
};
/*答案错误:您提交的程序没有通过所有的测试用例
case通过率为37.50%

用例:
"hello world"

对应输出应该为:

"hello%20%20world"

你的输出为:

"hello%20 20world"
*/
//是因为之后的长度我使用的是原来的长度加上空格乘2，但是原来的长度里面已经包含了空格长度，最后可能会致使总长度变多
//最后是因为忘了将str[j+k]=str[j];语句放在else里


```

