更改linux网络设置
=================

apt源
-----

apt源的配置文件位于 `/etc/apt/sources.list`。

以更换为阿里云的apt源为例，可以用以下命令来修改：

.. code-block:: bash

    sed -i 's|archive.ubuntu.com|mirrors.aliyun.com|g' /etc/apt/sources.list
    sed -i 's|security.ubuntu.com|mirrors.aliyun.com|g' /etc/apt/sources.list

上面的 `sed` 命令可以完成文件中所有匹配字符串的替换。

pip源
-----

用户的pip源配置文件位于 `~/.pip/pip.conf`，如果没有可以新建一个。

以更换为阿里云的pip源为例，`pip.conf` 内容如下：

.. code-block:: bash

    [global]
    index-url= http://mirrors.aliyun.com/pypi/simple/
    extra-index-url= https://pypi.python.org/simple
    trusted-host=mirrors.aliyun.com

系统默认的pip源配置文件位于 `/etc/pip.conf`，修改方法同上，该文件可以影响到所有用户。

设置代理
--------

通常用 `export` 指令来实现，如果需要长期保存可以加到 `.bashrc` 中去。

.. code-block:: 

    export http_proxy="http://localhost:port"
    export https_proxy="http://localhost:port"

DNS
---

DNS配置文件位于 `/etc/resolv.conf` 文件中，更改 ``nameserver`` 即可。

.. warning::

    这样只能临时修改DNS，重启后失效。

.. warning::

    方法来自百度，没有验证过。
