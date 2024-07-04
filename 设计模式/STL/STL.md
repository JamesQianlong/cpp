# STL

## 第一讲C++ Standard Library和Standard Template Library是同一个东西吗？

不是 C++标准库>STL,**STL分为六个部件(容器、算法、迭代器、适配器、仿函数、分配器)**，其外的是C++标准库范畴,STL以头文件(Header files)形式呈现  
**C++标准库的头文件不带副档名.h，例如#include<vector>**  
**新式的C头文件中不带副档名.h，例如#include<cstdio>**  
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
