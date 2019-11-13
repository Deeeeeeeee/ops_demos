# 主从复制

这里有两个配置文件 redis6380.conf 和 redis6381.conf，6380 是主节点，6381 是从节点。slave 配置文件 redis6381.conf 和 master 配置文件 redis6380.conf 的区别是

- 多了一行主从配置 `slaveof 192.169.0.1 6380`。由于我这里配置了地址为 192.169.0.1，这里需要大家自行修改
- 主节点密码 `masterauth 123456`

这两个节点的密码都是 `123456`，redis-cli 登陆的时候可以执行 `redis-cli -p 6380 -a 123456` 进行登录

# 使用

这里只描述使用 docker-compose 的方法，不使用 docker 的话，需要修改配置文件 redis6380.conf 里面 `port 6380`

- 首先需要安装好 docker 和 docker-compose
- 创建数据路径和给予执行权限
    ```
    $ mkdir 6380data & mkdir 6381data
    $ chmod a+rwx 6380data
    $ chmod a+rwx 6381data
    ```
- 启动容器
    ```
    $ docker-compose up -d
    ```

# 另外

容器之间的连接可以通过网桥进行连接，这也是我上面配置 `slaveof 192.169.0.1 6380` 的原因

```shell
# 创建 192.169.0.1 为网关的网桥
$ docker network create -d bridge --subnet 192.169.0.0/24 --gateway 192.169.0.1 localNet
```