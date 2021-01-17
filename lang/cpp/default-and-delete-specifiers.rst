.. _default和delete关键字:

default和delete关键字
====================

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
