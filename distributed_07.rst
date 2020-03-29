分布式部署文档 - guacamole 部署
----------------------------------------------------

说明
~~~~~~~
-  # 开头的行表示注释
-  $ 开头的行表示需要执行的命令

环境
~~~~~~~

-  系统: CentOS 7
-  IP: 192.168.100.50

+----------+------------+-----------------+---------------+------------------------+
| Protocol | ServerName |        IP       |      Port     |         Used By        |
+==========+============+=================+===============+========================+
|    TCP   | Guacamole  | 192.168.100.50  |      8081     |         Tengine        |
+----------+------------+-----------------+---------------+------------------------+
|    TCP   | Guacamole  | 192.168.100.51  |      8081     |         Tengine        |
+----------+------------+-----------------+---------------+------------------------+


开始安装
~~~~~~~~~~~~

.. code-block:: shell

    # 升级系统
    $ yum upgrade -y

    # 设置防火墙, 开放 8081 端口 给 Tengine 访问
    $ firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="192.168.100.100" port protocol="tcp" port="8081" accept"
    $ firewall-cmd --reload

    # 安装 docker
    $ sudo yum install -y yum-utils device-mapper-persistent-data lvm2
    $ yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    $ yum makecache fast
    $ yum -y install docker-ce wget
    $ mkdir /etc/docker
    $ wget -O /etc/docker/daemon.json http://demo.jumpserver.org/download/docker/daemon.json
    $ systemctl enable docker
    $ systemctl start docker

    # 通过 docker 部署
    $ docker run --name jms_guacamole -d \
        -p 8081:8080 \
        -e JUMPSERVER_KEY_DIR=/config/guacamole/key \
        -e JUMPSERVER_SERVER=http://192.168.100.100 \
        -e BOOTSTRAP_TOKEN=你的token \
        -e GUACAMOLE_LOG_LEVEL=ERROR \
        jumpserver/jms_guacamole:1.5.7

    # 访问 http://192.168.100.100/terminal/terminal/ 检查 guacamole 注册


多节点部署
~~~~~~~~~~~~~~~~~~

.. code-block:: shell

    # 登录到新的节点服务器, 前面安装 docker 都是一样的, 这里就懒得写了
    $ firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="192.168.100.100" port protocol="tcp" port="8082" accept"
    $ firewall-cmd --reload
    $ docker run --name jms_guacamole -d \
        -p 8081:8080 \
        -e JUMPSERVER_KEY_DIR=/config/guacamole/key \
        -e JUMPSERVER_SERVER=http://192.168.100.100 \
        -e BOOTSTRAP_TOKEN=你的token \
        -e GUACAMOLE_LOG_LEVEL=ERROR \
        jumpserver/jms_guacamole:1.5.7

    # 访问 http://192.168.100.100/terminal/terminal/ 检查 guacamole 注册
