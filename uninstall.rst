卸载文档
-------------

- 确保已经停止 jms koko guacamole 进程
- 确定数据已经处理完毕
- 此操作不可逆
- 请自行替换文中相关路径为你的实际环境路径

1. 正常部署的使用下面方式进行卸载

.. code-block:: shell

    $ rm -rf /opt/jumpserver  # jumpserver 目录
    $ rm -rf /opt/koko /opt/kokodir  # koko 目录
    $ rm -rf /opt/docker-guacamole  # guacamole 目录
    $ rm -rf /opt/py3  # python3 虚拟环境
    $ rm -rf /config  # guacamole client 目录
    $ rm -rf /etc/nginx/conf.d/jumpserver.conf

2. 参照极速安装或者使用 docker 部署组件的使用下面方式进行卸载

.. code-block:: shell

    $ rm -rf /opt/jumpserver  # jumpserver 目录
    $ rm -rf /opt/py3  # python3 虚拟环境
    $ rm -rf /etc/nginx/conf.d/jumpserver.conf

    # 删除 docker 组件
    $ docker rm jms_koko
    $ docker rm jms_guacamole
    $ docker rmi jumpserver/jms_koko:1.5.8  # 自行替换版本
    $ docker rmi jumpserver/jms_guacamole:1.5.8  # 自行替换版本

    # 删除自启文件
    $ rm -rf /usr/lib/systemd/system/jms.service
    $ rm -rf /opt/start_jms.sh
    $ rm -rf /opt/stop_jms.sh

    # 删除数据库
    $ mysql -uroot
    > drop database jumpserver;
    > exit

    # 清空 redis
    $ redis-cli -h 127.0.0.1
    > flushall
    > exit

    $ reboot # 重启服务器
