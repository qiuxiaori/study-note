## Docker实战之mongo

> 使用 docker 运行 mongo，可以省去安装 mongo 数据库的步骤， docker也提供有mongo的镜像，不过还是有许多要配置的地方。

### 拉取镜像

* 搜索查看镜像：`docker search mongo` 基本上第一个就是了
* 拉取镜像：`docker pull mongo`

### 运行容器
* 普通模式运行：
  * 执行 `docker run --name <YOUR-NAME> -p 27017:27017 -v /data/db:/data/db -d mongo:3.4`
  * 普通模式启动 mongo 容器后就可以直接连接 mongodb://127.0.0.1:27017/dbname 操作数据库了
* auth模式运行：
  * `docker run --name <YOUR-NAME> -p 27017:27017 -v /data/db:/data/db -d mongo:3.4 -auth`
  * 管理员模式启动后需要添加用户管理员已经数据库管理员才能操作，详见第三四章节
* 参数说明
  * —name： 指定库的名字，如果不指定会使用一串随机字符串
  * -p 27017:27017： 官方的镜像已经暴露了 27017 端口，我们将它映射到主机的端口上。如果你不使用默认端口，将 : 前面的数字改成自定义端口
  * -v /data/db:/data/db： 冒号前面的是主机上的文件路径，将它挂载到库中的文件夹下，实际对文件的读写就会在主机文件上操作
  * -d： 在后台运行
  * mongo:3.4 指定镜像版本，默认是 latest 。建议总是自己指定版本
  * —auth 以 auth 模式运行 mongo
* 执行一下 `docker ps` 确认 mongo 已经运行起来

### 添加用户管理员
> auth模式运行的数据库先需要添加一个管理用户的用户管理员
* 执行 `docker exec -it <YOUR-NAME> mongo admin` 进入到 shell 中
* 添加管理员 `db.createUser({ user: 'username', pwd: 'password', roles: [ { role: 'userAdminAnyDatabase', db: 'admin' } ]})`
* 之后用管理员身份登录可以：
  * 方式一： `docker exec -it <YOUR-NAME> mongo -u <USER> -p <PASSWORD> --authenticationDatabase admin`
  * 方式二：
  ```js
  docker exec -it <YOUR-NAME> /bin/bash // 进入到 shell
  use admin // 使用 admin 数据库
  db.auth(username, password) // 登入
  ```

### 数据库权限
> 刚刚添加的管理员只是mongo数据库中用来管理其他用户的管理员，如果相对数据进行操作，需要创建数据库管理员
* 以用户管理员登入 shell
* 切换到要添加管理的数据库：`use <DbName>`
* 新建：`db.createUser({ user: 'username', pwd: 'password', roles: [ { role: 'root', db: <DbName> }, { role: 'readWrite', db: <DbName> } ]})`
* 以这个账户连接 mongo 就具有读写权限了

### 参考文章
1. [利用 Docker 运行 MongoDB](ttps://brickyang.github.io/2017/03/15/%E5%88%A9%E7%94%A8-Docker-%E8%BF%90%E8%A1%8C-MongoDB/)
2. [MongoDB 基础(六)安全性(权限操作)](https://blog.csdn.net/kk185800961/article/details/45619863)

> [原文博客地址](https://qiuxiaori.github.io/xr-blog/)