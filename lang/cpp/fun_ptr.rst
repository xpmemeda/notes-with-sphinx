函数指针
========

函数也是一个对象（个人理解），也可以用指针来指向它。

由于函数的类型只有编译器知道，因此函数指针的声明方式与普通对象不同，形式如下：

.. code-block:: cpp

    int foo() {
        return 5;
    }
    
    int goo() {
        return 6;
    }
    
    int main() {
        int (*funcPtr)() = &foo;  // funcPtr 现在指向了函数foo
        funcPtr = &goo;  // funcPtr 现在又指向了函数goo
        return 0;
    }

函数指针在赋值时，必须保证其声明的参数和返回值与等号右边的函数相同：

.. code-block:: cpp

    int foo();
    double goo();
    int hoo(int x);
    
    // 给函数指针赋值
    int (*funcPtr1)() = &foo;  // 可以
    // int (*funcPtr2)() = &goo;  // 错误！返回值不匹配！
    double (*funcPtr4)() = &goo;  // 可以
    // funcPtr1 = &hoo;  // 错误，参数不匹配
    int (*funcPtr3)(int) = &hoo; // 可以

可以通过函数指针来调用该函数：

.. code-block:: cpp

    int foo(int x) {
        return x;
    }
    
    int main() {
        int (*funcPtr)(int) = &foo; 
        (*funcPtr)(5);  // 通过funcPtr调用foo(5)
        funcPtr(5);  // 也可以这么使用，在一些古老的编译器上可能不行
        return 0;
    }

把函数作为参数传入另一个函数:

.. code-block:: cpp

    #include <iostream>

    int add(int a, int b) {
        return a+b;
    }

    int sub(int a, int b) {
        return a-b;
    }

    void func(int e, int d, int(*f)(int a, int b)) {
        std::cout<<(*f)(e,d)<<std::endl;
    }

    int main() {
        func(2 , 3, &add);
        func(2 , 3, &sub);
        return 0;
    }

.. note::

    1. 函数指针在赋值时，等号右边的函数可以不加 ``&`` 符号。

    2. 函数指针在使用时，指针左边可以不加 ``*`` 符号。