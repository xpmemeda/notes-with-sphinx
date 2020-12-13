检查代码格式
============

isort
-----

isort可以按照字母顺序为所有import内容排序，并且自动分成五个不同的区域，他们是 ``FUTURE,STDLIB,THIRDPARTY,FIRSTPARTY,LOCALFOLDER``。

通常来说，只要直接在命令行输入 `isort file_directory` 就可以使用默认设置达到目的。

如果想要更改默认行为，可以在 `setup.cfg` 中添加内容，例如把megengine视为一个三方库。

:: 

   extra_third_party = megengine

官方详细文档地址 `isort <https://pypi.org/project/isort/>`_。

black
-----

black是一个代码格式化工具，快速使用方法为 `black file_directory`。

官方详细文档地址 `black <https://pypi.org/project/black/>`_。

CI中的配置方法
--------------

.. code-block:: shell

    #!/usr/bin/env bash
    set -e
    cd $(dirname $0)/..

    ISORT_ARG=""
    BLACK_ARG=""

    while getopts 'd' OPT; do
        case $OPT in
            d)
                ISORT_ARG="--diff --check-only"
                BLACK_ARG="--diff --check"
                ;;
            ?)
                echo "Usage: `basename $0` [-d]"
        esac
    done

    isort $ISORT_ARG -j $(nproc) -rc .
    black $BLACK_ARG --target-version=py36 -- .
