.. _变参模板:

变参模块
========

变参模板（variadic templates）是C++11新增的最强大的特性之一。

一个简单的可变模版参数函数：

.. code-block:: cpp

    template <class... T>
    void f(T... args)
    {    
        cout << sizeof...(args) << endl;  // 打印变参的个数
    }

    f();              // 0
    f(1, 2);          // 2
    f(1, 2.5, "");    // 3

递归函数方式展开参数包
---------------------

通过递归函数展开参数包，需要提供一个参数包展开的函数和一个递归终止函数。

以求和为例：

.. code-block:: cpp

    template<typename T>
    T sum(T t)
    {
        return t;
    }
    template<typename T, typename ... Types>
    T sum (T first, Types ... rest)  // 把第一个参数提取出来
    {
        return first + sum(rest...);
    }
    int main(){
        sum(1,2,3,4);  // 10
        return 0;
    }

.. note::

    参考自 `泛化之美--C++11可变模版参数的妙用 <https://www.cnblogs.com/qicosmos/p/4325949.html>`_。