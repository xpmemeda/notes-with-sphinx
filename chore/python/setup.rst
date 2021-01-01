通过pip指令安装自己的python包
============================

pip安装流程简介
--------------

如果想让自己的python库可以通过 ``pip3 install . --user`` 来安装，那么需要编写一个 `setup.py` 文件。

``pip3 install . --user`` 其实会分两个步骤来执行。

1. 创建一个临时目录，执行 `setup.py` 里面的指令，并把指定包目录的代码拷贝至 `<build_dir>/lib.xxx` 中。
2. 把第一步产生的 `<build_dir>/lib.xxx` 移动至 `~/.local/lib/python3.6/site-packages/` 路径下。

``pip3 install . --user`` 与下面的两条指令是等价的，分别对应上面两步，而这时候的 `<build_dir>` 就是 `./build/`。

.. code-block:: bash

    python3 setup.py build
    python3 setup.py install --user

如何编写setup.py
----------------

如果代码库不需要编译，那么直接调用 ``setuptools`` 包里面的 ``setup`` 函数即可，需要指明 `name` ， `version` 和 `packages` 等参数。

如果需要编译，那么需要自定义extension和builder，详细信息可以参考 pybind11 example 的 `setup.py <https://github.com/xpmemeda/pybind11-examples/blob/master/setup.py>`_。

.. note::

    ``self.build_temp`` 指的是编译的临时目录。

    ``self.build_lib`` 指的是生成的包的路径，也就是上面的 ``<build_dir>/lib.xxx``。
