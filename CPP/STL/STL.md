# STL

## 第一讲C++ Standard Library和Standard Template Library是同一个东西吗？

不是 C++标准库>STL,**STL分为六个部件(容器、算法、迭代器、适配器、仿函数、分配器)**，其外的是C++标准库范畴,STL以头文件(Header files)形式呈现  
**C++标准库的头文件不带副档名.h**  
**新式的C头文件中不带副档名.h**  
**旧式C头文件仍然可用，例如#include<stdio.h>**  
新式headers内的组件封装于namespace std 可以使用using namespace std或者using std::cout  
旧式headers内的组件不封装于namespace std  
**三个网站**
Cplusplus.com cppreference.com gcc.gnu.org  

## 容器的六大部件：容器、算法、迭代器、适配器、仿函数、分配器  

```c++
#include<vector>
#include<algorithm>
#include<functional>
#include<iostream>
using namespace std;
int main(){
    int ia[6] ={27,210,12,47,109,83};
    vector<int,allocator<int>> vi(ia,ia+6); //容器 分配器
    cout<<count_if(vi.begin(),vi.end(),not1(bind2nd(less<int>(),40))); //算法
    return 0;
}
```

## 前闭后开区间

在迭代器中，一般使用前闭后开规则(一般容器的begin()方法指向第一个元素的位置，end()方法指向最后一个元素的下一个位置)  

```c++
Container<T> c;
Container<T>::iterator ite = c.begin();
for(;ite!=c.end();ite++)
```
## 容器

在STL中，容器一般可以被分为三类：顺序容器(Sequence containers)、关联容器(Associative containers)、不定序容器(Unordered containers) 

### 顺序容器
Array:简单的定长数组，长度不可变。**数组存在一个连续空间上**。
Vector:变长数组，或者叫向量。可以用push_back操作在尾部扩充。vector也是存储在连续空间上。**vector存储的空间大小只会是2的指数。如果当前的空间不够用了，就会重新开辟一个新的大小为2n的空间，然后复制原来空间中的值**。  
Deque:deque是双端队列。其两端都可以删除或者增加。**deque从逻辑上是线性的，但是实际上，他是一个map,可以看作是一个链表数组，在物理上是分段的**。也就是维持着多个buffer,这些buffer在逻辑上连续。这样才能做到两端都可以扩充。deque这样做可以减少容量的浪费。它也是stack和queue的缺省实现  
list:双向链表，在内存中不一定是连续的.list和forward_list有自己的.sort成员函数，针对这种存储方式进行了优化。所以应该使用这个函数。  
forward_list:单向链表。cpp11的新标准，以前有GNU的slist。少了前向指针可以减少内存占用  
stack和queue:这俩可以说是容器，也可以说是container adapter(适配器)，因为其底层是由deque/vector/list实现的。他们不提供iterater（迭代器）的操作,一般有自带的find函数。 

### 关联容器
这类容器一般是用红黑树（一种自平衡的二叉搜索树）实现的，一般用于查找某些元素。其中元素顺序与其值的大小相关，目的是为了提供更快的查找顺序。可以分为set/multiset、map/multimap。  


### 不定序容器
unordered container：特殊的associative container，用哈希表(拉链法)存储元素，因此其是无序的。可以分为unordered_set、unordered_map。  