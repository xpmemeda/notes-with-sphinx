简易使用流程
============

1. 添加github上的pybind11为子仓库

.. code-block:: shell

    git submodule add https://github.com/pybind/pybind11

2. 修改的cpp源文件，封装需要绑定的变量、函数和类。

3. 编写CMakeLists.txt文件，比如这样：

.. code-block:: cmake

    cmake_minimum_required(VERSION 3.10)
    project(pybind11-examples)
    add_subdirectory(pybind11)
    pybind11_add_module(${PROJECT_NAME} src/)