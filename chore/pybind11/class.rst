绑定自定义类
===========

向python提供C++中的自定义类型，可以参考 `pybind11官方文档 <https://pybind11.readthedocs.io/en/stable/classes.html>`_。

然而C++中实现的类并非一定要在python代码中调用其成员变量和函数，在部分场景下，它作为参数提供给pybind11封装的C++接口，这时候只需要在pybind11编写库的时候定义该类即可。

.. code-block:: cpp

    #include <pybind11/pybind11.h>
    #include <iostream>

    namespace py = pybind11;

    struct A {
        void print() {
            std::cout << "a..." << std::endl;
        }
    };

    void pa(A a) {
        a.print();
    }

    A ga() {
        return A();
    }

    PYBIND11_MODULE(_cambriconLib, m) {
        m.def("add", [](double x, double y) -> double { return x + y; });
        py::class_<A>(m, "A");
        m.def("ga", &ga);
        m.def("pa", &pa);
    }

.. warning::

    - cpp文件中必须有该类的完整定义
    - 绑定时必须声明该类，否则python无法识别返回类型
