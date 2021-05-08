# 一、基础



# 二、标准库

## 1、IO库

```c++
iostream fstream sstream
    
istringstream：
eg1：
    bool judge_ip(string ip){
    istringstream iss(ip);
    string seq;
    int j = 0;
    while (getline(iss,seq,'.')){
        if(++j>4 || seq.empty() || stoi(seq)>255){return false;}
    }
    return true;
    }
eg2：
    while(getline(cin,line)){
        int info;
        istringstream record(line);
        record >> info;
        while (record >> word)//word是string
            phones.push_back(word);
    }
```

## 2、顺序容器

### 2.1 概述

![image-20210222093028957](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222093028957.png)

内存开销大  -> 不使用 list/forward_list            随机访问元素 -> vector/deque

中间插删元素 -> list/forward_list                      头尾插删元素 -> deque

### 2.2 容器库操作

![image-20210222093910516](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222093910516.png)

![image-20210222093939455](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222093939455.png)

![image-20210222093953530](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222093953530.png)

### 2.3 容器定义、初始化、赋值

![image-20210222094351897](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222094351897.png)

![image-20210222094621606](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222094621606.png)

### 2.4 操作

![image-20210222094757160](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222094757160.png)

![image-20210222095313308](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222095313308.png)

![image-20210222095330420](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222095330420.png)

![image-20210222095622029](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222095622029.png)

### 2.5 string

![image-20210222095837821](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222095837821.png)

![image-20210222095906965](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222095906965.png)

![image-20210222095929650](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222095929650.png)

![image-20210222095945892](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222095945892.png)

![image-20210222100006558](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222100006558.png)

![image-20210222100018267](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222100018267.png)

![image-20210222100043584](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222100043584.png)

### 2.6 数值转换

![image-20210222100156856](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222100156856.png)

## 3、关联容器

![image-20210222100538887](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222100538887.png)

### 使用map：

![image-20210222101023322](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222101023322.png)

### 使用set:

![image-20210222101101034](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222101101034.png)

### 定义

![image-20210222101300190](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222101300190.png)

![image-20210222101427415](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222101427415.png)

![image-20210222101453943](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222101453943.png)

![image-20210222101514663](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222101514663.png)

![image-20210222101603591](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20210222101603591.png)

## 4、动态内存

智能指针： 更容易（更安全）地使用动态内存。行为类似于常规指针，区别在于负责自动释放所指向的对象。

shared_ptr 允许多个指针指向同一个对象； unique_ptr “独占”所指向的对象； weak_ptr 弱引用，指向shared_ptr所管理的对象



# 三、类设计

## 3 模板类

```c++
#include <iostream>
 
template <int X>
struct A
{
    static const int value = A<X - 1>::value + X;
};
 
template <>
struct A<1>
{
    static const int value = 1;
};
 
int main()
{    std::cout << A<10>::value << std::endl;  }

//#2
template <int N>
struct Factorial 
{    enum { value = N * Factorial<N - 1>::value };  };
  
template <>
struct Factorial<0> 
{    enum { value = 1 };  };
  
// Factorial<4>::value == 24
// Factorial<0>::value == 1
void foo()
{
    int x = Factorial<4>::value; // == 24
    int y = Factorial<0>::value; // == 1
}
```



# 四、其他



