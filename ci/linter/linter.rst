静态代码检查
============

pylint
------

Pylint是一个Python静态代码分析工具，官方文档地址 `pylint <https://pypi.org/project/pylint/>`_。

mypy
----

mypy也是一个静态代码分析工具，通常用来辅助pylint以获得更强大的检查能力。

CI中的配置方法
-------------

.. code-block:: shell

    set -e
    cd $(dirname $0)/..

    mypy mgeconvert test --show-error-codes --ignore-missing-imports || mypy_ret=$?
    pylint mgeconvert test --rcfile=.pylintrc || pylint_ret=$?

    if [ "$mypy_ret" ]; then
        exit $mypy_ret
    fi

    if [ "$pylint_ret" ]; then
        exit $pylint_ret
    fi
