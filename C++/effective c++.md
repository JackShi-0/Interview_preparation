# 2、面向过程

###### #include  ‘’ ‘’ 和 <>

“ ” ：头文件和包含此文件的程序代码文件位于同一磁盘目录下，被认为是一个用户提供的头文件，搜索此文件时，是从该所在磁盘目录开始搜起；

<>：在不同的的磁盘目录下，被认定是标准的或项目专属的头文件，会从默认的磁盘目录开始寻找。

###### 

# 4、基于对象的编程风格（class的设计与实现）

```c++
class stack{
    public: //可以在程序的任何地方被访问  //操作函数和运算符(成员函数) //声明，若在class里定义，自动被视为inline函数
    
    protected: //其所有成员都可以被派生类直接访问
    
    private: //只能在成员函数和class friend中被访问 //实现细节
}
```

## 构造函数（特别的初始化函数）

```c++
class triangular{
 public:  triangular();
          triangular(int len);
          triangular(int len, int beg_pos);
 private: int _length, _beg_pos， _next;
};
//初始化1
  //1、不接受任何参数：
   triangular:: triangular(){_length=1; _beg_pos=1; _next=0;}
  //2、每个参数提供默认值
   class triangular{
       public: triangular(int len=1, int bp=1);
   };
   triangular:: triangular(int len, int bp){
       _length=1en>0? len:1;
       _beg_pos=bp>0? bp:1;
       _next=_beg_pos-1;
   }
//初始化2 ///成员初始化列表
class triangular{
 public:  //
 private: string _name; int _length, _beg_pos, _next;
};
triangular::triangular(int len, int bp)
    : _name("triangular"); //_name初值
{ //... 
}

```

## 析构函数（Destructor）

加上”~“，绝对不会有返回值，也没有任何参数。参数列表为空，也不会被重载。

编译器会在定义mat时，暗暗使用构造函数；语句块结束前，又会暗暗应用析构；C++最难的即是，何时需要定义Destructor.

//注意初始化，考虑是否需要copy constructor 类里面有没有new在heap里

```c++
#include <iostream>
using namespace std;
class A{
public:
    virtual void F(){cout << 1 << endl;}
    void CallF(){F();}
    virtual ~A(){CallF(); F();}
};

class B : public A{
public:
    void F(){cout << 2 << endl;}
    ~B(){}
};

class C : public B{
public:
    void F(){cout << 3 << endl;}
    void CallF(){F(); A::CallF();}
    ~C(){CallF();}
};

int main(){
    A * p = new C();
    p->CallF();
    delete p;
    return 0;
}
```

## this指针

在成员函数(member function)内用来指向其调用者（一个对象）。内部工作过程：编译器自动将this指针加到每一个member function的参数列表。

```c++
trianglar& triangular::
copy( const triangular &rhs)
{
    if(this != &rhs){
        _length = rhs._length; _beg_pos = rhs._beg_pos;
    }
    return *this;
}
```

## const & mutable

```c++
class triangular{
public： int length()        const {return _length;}
         int elem( int pos)  const;   //const member function
         bool next(int &val);         //non-const member function
private: //....    
};
//class体外定义const，必须在声明与定义中指定const
int triangular：：elem(int pos) const
{return _elems[pos-1];}
//mutable 
//关键字，对外宣称对该参数的改变，不改变类目标的常量性。
```



## 静态类成员

static data member用来表示**唯一的、可共享的member，可以在同一类的所有对象中被访问**。

一般情形的成员函数调用，必须通过其类的某个对象调用：该对象会被绑定至这个成员函数的this指针，通过存储于每个对象的this指针，成员函数才能访问储存于每个对象中的非静态数据成员。

static member function(静态成员函数)：直接访问：if( `Triangular :: is_elem(8)`), 可以在”与任何对象都无瓜葛“的情形下被调用。Tips：成员函数只有在”不访问任何非静态成员“的条件下才能被声明为static。

## 迭代器类

运算符函数不用指定名称，只需在运算符前加上关键字`operator`

## friend

具备了与class member function相同的访问权限，可以访问class的private member。

## Pointer to member function

必须通过同一类的对象加以调用，对象即为成员函数中`this`指针所指之物。

# 5、面向对象编程（继承、多态）

继承：将一群相关的类组织起来，让我们得以分享其间的共通数据和操作行为；

多态：当我们在对类进行编程时，可以如同操作单一个体，而非相互独立的类，赋予更多弹性来加入或移除任何特定类。//让基类的**pointer或reference**得以十分透明的指向其任何一个派生类的对象；

动态绑定：执行之前不知道调用哪个函数，解析操作会延迟至运行时进行。

## #5.2 ***核心

## #5.3 定义抽象基类

1、找出所有子类共通的操作行为； //操作行为的合集：基类的公有接口；

2、找出与类型相关的操作行为（必须根据不同的派生类而有不同的实现方式），定义为虚函数；static成员函数不能被定义为虚函数；

3、找出每个操作行为的访问层级，public(皆能访问)，基类以外不需要用到private（基类的派生类也无法访问），protected（可让派生类访问，一般程序不能访问）；

### 纯虚函数

接口的不完整性（没有函数定义），只能作为派生类的子对象使用，派生类必须为所有虚函数提供确切的定义。

## #运用继承体系

## #基类的另一种设计方式

将所有派生类共有的实现内容剥离出来，移至基类内。

```c++
class num_sequence{
public: 
    virtual ~num_sequence(){};
    virtual const char* what_am_i() const = 0;
    int           elem(int pos) const;  //
    ostream&      print(ostream &os = cout) const;///重新定义
    
    int           length() const {return _length;} //
    int           beg_pos() const{return _beg_pos;}//提升至public,都能读取
    static int    max_elems()    {return 64;}

protected:
    virtual void  gen_elems(int pos) const = 0;
    bool          check_integrity(int pos, int size) const;
    
    num_sequence(int len, int bp, vector<int> &re)
        : _length(len),_beg_pos(bp),_relems(re){}
    
    int           _length;//
    int           _beg_pos;//提升至protected(派生类可访问)：num_sequence扮演的是每个派生类的子对象(原因)
    vector<int>   & _relems;///新增的data member，指向派生类的某个static vector///使用引用，永远无法代表空对象，不必检查是否为null///引用要在成员初始化列表纪念性初始化，再也无法指向另一个对象
};///依然是个抽象基类，但现在能提供一些实现内容，可供各派生类继承

//派生类只需编写自身独特的部分
class fibonacci : public num_sequence{
public: 
    fibonacci(int len=1, int beg_pos=1);
    virtual const char* what_am_i() const {return "fibonacci";}
protected:
    virtual void   gen_elems(int pos) const;
    static vector<int>  _elems;
};
```

## #初始化、析构、复制

初始化较好的设计方式：为基类提供constructor，并利用这个constructor处理基类所声明的所有data member的初始化操作。

复制：基类子对象会被逐一初始化，然后派生类的member亦会被逐一初始化。

析构：基类的destructor会在派生类的destructor结束后被自动调用，无需在派生类中对它进行明确的调用操作。

## #派生类中定义一个虚函数

定义派生类：

1、原封不动地加以继承。继承了纯虚函数，那么派生类会被视为抽象类，无法为它定义任何对象；

2、覆盖基类所提供的虚函数，派生类提供的新定义的函数原型必须完全符合基类所声明的函数原型（参数列表、返回类型、常量性）。常见错误：派生类的声明未完全吻合基类中的声明。----例外：当基类的虚函数返回某个基类形式（指针或引用）。

## #类型鉴别机制

1、只提供唯一的一份`what_am_i`

2、运行时类型鉴别机制（`RTTI`）: `typeid`运算符 头文件 `typeinfo`

类型转换：

static_cast: 潜在危险，编译器无法确认进行的转换操作是否完全正确；

dynamic_cast: 进行运行时检验操作，只适用于指针或引用。

# 6、以`template`进行编程

```c++
//1
template <typename Type>
class BinaryTree;

template <typename valType>
class BTnode{
    friend class BinaryTree<valType>;
public:    
private:
    valType _val;
};
//2
template <typename elemType>
class BinaryTree{
public:
private:
    BTnode<elemType> *_root;
};
```

# 7、异常处理