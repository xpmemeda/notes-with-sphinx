.. _初始化列表:

初始化列表
==========

C++11允许构造函数和其他函数把初始化列表当做参数，举例如下：

.. code-block:: cpp

    class CompareClass 
    {
    CompareClass (int,int);
    CompareClass (initializer_list<int>);
    };

    int main（）
    {
        myclass foo {10,20};  // calls initializer_list ctor
        myclass bar (10,20);  // calls first ctor 
    }
