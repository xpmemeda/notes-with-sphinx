.. _新增关键字:

C++11新增关键字
==============

noexcept
--------

放在函数声明的后面，表示这个函数不会抛出异常，从而令编译器可以做更多优化。
在 C++11 之前，这个声明由 ``throw()`` 来实现。

.. code:: cpp

    void swap(Type& x, Type& y) throw()   // C++11 之前
    {
        x.swap(y);
    }
    void swap(Type& x, Type& y) noexcept  // C++11 以后
    {
        x.swap(y);
    }

decltype
--------

这个关键字被用来在编译期间推断变量类型，当然也可以为我们编写代码提供方便。

.. code:: cpp

    int x = 4;
    decltype(x) y;  // 推断结果为 int，a 的类型为 int。

1. 结合 ``using`` 和 ``typedef`` 可以用于类型定义：

.. code:: cpp

    vector<int >vec;
    typedef decltype(vec.begin()) vectype;
    for (vectype i = vec.begin; i != vec.end(); i++)
    {
        //...
    }

2. 重用匿名类型

.. code:: cpp

    struct 
    {
        int d ;
        doubel b;
    }a;

    decltype(a) b;  // 定义了一个上面匿名的结构体

auto
----

.. note::

    ``auto`` 关键字不允许没有初始值的声明，比如：

.. code:: cpp

    int x;
    auto y = x;  // ok
    auto z;      // error

1. 用来自动推断类型，从而节省很多代码。

.. code-block:: cpp

    std::vector<int> v;
    std::vector<int>::iterator i1 = v.begin();
    auto i2 = v.begin();

2. 配合 lambda 表达式类使用，每个 lambda 表达式的类型都是独一无二的，这个类型只有编译器知道，因此创建时需要使用 ``auto`` 关键字。

.. code:: cpp

    auto closure = [](const int&, const int&) {}

虽然也可以使用 ``function`` 来实现，但这远没有 ``auto`` 来的简单直观。

3. 模板函数返回值

.. code:: cpp

    template<class T, class U>
    auto mul(T x, U y) -> decltype(x * y)
    {
        return x * y;
    }

.. note::

    在 C++14 之后，可以省去后面的 ``decltype(x * y)``。

default和delete
--------------

- default

创建类时，如果自己提供了任何形式的构造函数，那么编译器将不会产生默认构造函数。
``default`` 关键字则告诉编译器产生一个默认构造函数，如：

.. code-block:: cpp

    class A
    {
    public:
        A(int a){}
        A() = default;
    };

- delete

禁用被 ``delete`` 修饰的函数签名。

.. code-block::

    class A
    {
    public:
        A(int a){};
        A(double) = delete;         // conversion disabled
        A& operator=(const A&) = delete;  // assignment operator disabled
    };
    int main{
        A a(10);     // OK
        A b(3.14);   // Error: conversion from double to int disabled
        a = b;       // Error: assignment operator disabled
    }

override和final
==============

如果派生类在虚函数声明时使用了 ``override`` 描述符，那么该函数必须重载其基类中的同名虚函数，否则代码将无法通过编译。
要求函数名相同，参数一致，且基类中的该函数是虚函数。

``final`` 禁用类的继承和函数的重写。

.. code:: cpp

    class Super final
    {
    //......
    };

.. code:: cpp

    class Super
    {
    public:
        Supe();
        virtual void SomeMethod() final;
    };
