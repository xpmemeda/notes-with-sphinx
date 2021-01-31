std::tuple
==========

``std::tuple`` 定义在头文件 ``<tuple>`` 中。

基本用法
--------

1. 初始化：该结构可以组合多种类型，初始化列表中必须包含其声明的所有项。

 - ``std::tuple`` ：必须声明模板类型。

 - ``std::make_tuple`` ：可以自动推导返回类型。

2. 提取内容

 - ``std::tie`` ：提取所有内容，不需要的项目可以使用 ``std:ignore`` 来代替。

 - ``std::get`` ：提取单个内容，需要索引，返回该对象的引用，因此可以修改。

.. code-block:: cpp

    #include <iostream>     // std::cout
    #include <tuple>        // std::tuple, std::get, std::tie, std::ignore

    int main () {
    std::tuple<int,char> foo (10,'x');
    auto bar = std::make_tuple ("test", 3.1, 14, 'y');

    std::get<2>(bar) = 100;                                    // access element

    int myint; char mychar;

    std::tie (myint, mychar) = foo;                            // unpack elements
    std::tie (std::ignore, std::ignore, myint, mychar) = bar;  // unpack (with ignore)

    return 0;
    }

std::apply
---------

``<tuple>`` 中提供了一个类似于 ``std::invoke`` 的函数 ``std::apply`` ，区别在于后者可以把 ``std::tuple`` 类型的变量展开。

.. code-block:: cpp

    int f(int, int);
    int g(tuple<int, int>);

    std::tuple<int, int> tup(1, 2);

    std::invoke(f, 1, 2); // calls f(1, 2)
    std::invoke(g, tup);  // calls g(tup)
    std::apply(f, tup);   // also calls f(1, 2)
    // std::invoke(f, tup);  // 错误，参数不匹配
