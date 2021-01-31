.. _function:

std::function
=============

相比于函数指针， ``std::function`` 提供了更加简便的使用方式，同时支持使用Lambda表达式来保存运行时状态，功能更加强大。

基础用法
-------

``std::function`` 的实例可以对任何可以调用的目标实体进行存储、复制、和调用操作，这些目标实体包括普通函数、Lambda表达式、函数指针、以及其它函数对象等。

通常 ``std::function`` 是一个函数对象类，它包装其它任意的函数对象，被包装的函数对象具有类型为T1, …,TN的N个参数，并且返回一个可转换到R类型的值。

.. code-block:: cpp

    #include <functional>
    #include <iostream>
    using namespace std;

    std::function< int(int)> Functional;

    // 普通函数
    int TestFunc(int a)
    {
        return a;
    }

    // Lambda表达式
    auto lambda = [](int a)->int{ return a; };

    // 仿函数(functor)
    class Functor
    {
    public:
        int operator()(int a)
        {
            return a;
        }
    };

    // 1.类成员函数
    // 2.类静态函数
    class TestClass
    {
    public:
        int ClassMember(int a) { return a; }
        static int StaticMember(int a) { return a; }
    };

    int main()
    {
        // 普通函数
        Functional = TestFunc;
        int result = Functional(10);
        cout << "普通函数："<< result << endl;

        // Lambda表达式
        Functional = lambda;
        result = Functional(20);
        cout << "Lambda表达式："<< result << endl;

        // 仿函数
        Functor testFunctor;
        Functional = testFunctor;
        result = Functional(30);
        cout << "仿函数："<< result << endl;

        // 类成员函数
        TestClass testObj;
        Functional = std::bind(&TestClass::ClassMember, testObj, std::placeholders::_1);
        result = Functional(40);
        cout << "类成员函数："<< result << endl;

        // 类静态函数
        Functional = TestClass::StaticMember;
        result = Functional(50);
        cout << "类静态函数："<< result << endl;

        return 0;
    }

.. warning::

    关于可调用实体转换为std::function对象需要遵守以下两条原则： 

    1. 转换后的 ``std::function`` 对象的参数能转换为可调用实体的参数。

    2. 可调用实体的返回值能转换为 ``std::function`` 对象的返回值。

使用 ``std::bind`` 进行绑定
--------------------------

``std:bind`` 可以返回一个 ``std::function`` 对象，主要有两个用途：

1. 添加默认值

2. 绑定类成员函数

绑定普通函数
''''''''''''

.. code-block:: cpp

    int add (int x, int y) { return x + y; }
    int main() {
        auto fn = std::bind(add, std::placeholders::_1, 2);  
        std::cout << fn(2, 3) << std::endl;    
    }

绑定类成员函数
''''''''''''''

.. code-block:: cpp

    struct Foo {
        void print_sum(int x, int y) {
            std::cout << x + y << std::endl;
        }
    };

    int main()  {
        Foo foo;
        auto f = std::bind(&Foo::print_sum, &foo, 95, std::placeholders::_1);
        f(5);  // 100
    }