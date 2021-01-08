环境变量之系统路径
=================

- PATH

可执行文件的路径。

- LIBRARY_PATH

程序编译期间查找动态链接库时指定的其他共享库的路径。

- LD_LIBRARY_PATH

程序加载运行期间查找动态链接库时指定其他路径。

- C_INCLUDE_PATH

C程序编译期间查找头文件时指定的其他路径。

- CPLUS_INCLUDE_PATH

C++程序编译期间查找头文件时指定的其他路径。

- CPATH

C和C++程序在编译期间查找头文件时指定的其他路径。

配置cuda
========

通常来说，使用第三方硬件需要安装SDK后配置以上几个环境变量，令其可以在编译时成功 ``include``，在运行时找到库文件。

以cuda为例编写自动化配置脚本：

.. code-block:: bash

    #!/bin/bash -e

    function on_error() {
        echo  $1
        exit -1
    }

    for i in $(readlink -f $(dirname $0))/*; do
        [ -d $i ] || continue
        i=$(readlink -f $i)
        if [[ $(basename $i) =~ cuda-[10.]*$ ]]; then
            if [ -z "$cuda" ]; then
                cuda=$i
            else
                on_error "multiple cuda found"
            fi
        fi
        [ -d $i/include ] || on_error "$i/include not existing"
        INCL=$INCL:$i/include
        lib=$i/lib
        [ -d $lib ] || lib=$i/lib64
        [ -d $lib ] || on_error "$lib not existing"
        LIB=$LIB:$lib
    done

    LIB=${LIB#:}
    INCL=${INCL#:}

    [ -z "$cuda" ] && on_error "cuda not found"

    NEW_CPATH=$CPATH:$INCL
    NEW_CPATH=${NEW_CPATH#:}
    NEW_LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$LIB
    NEW_LD_LIBRARY_PATH=${NEW_LD_LIBRARY_PATH#:}
    NEW_LIBRARY_PATH=$LIBRARY_PATH:$LIB
    NEW_LIBRARY_PATH=${NEW_LIBRARY_PATH#:}

    echo "CPATH=$NEW_CPATH"
    echo "LD_LIBRARY_PATH=$NEW_LD_LIBRARY_PATH"
    echo "LIBRARY_PATH=$NEW_LIBRARY_PATH"
    echo "PATH=$PATH:$cuda/bin:$cuda/NsightCompute-2019.1"
    echo "CUDA_HOME=$cuda"
