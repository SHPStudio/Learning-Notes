# 使用yum安装
## 先安装yum
  ```access transformers
$ sudo yum install -y yum-utils \
           device-mapper-persistent-data \
           lvm2
```
## 向yum中添加源
```access transformers
$ sudo yum-config-manager \
    --add-repo \
    https://mirrors.ustc.edu.cn/docker-ce/linux/centos/docker-ce.repo
```
## 安装最新的docker-ce
```access transformers
sudo yum-config-manager --enable docker-ce-edge
```

## 安装docker-ce
```access transformers
$ sudo yum makecache fast
$ sudo yum install docker-ce
```

  这种安装需要一堆的步骤才可以，所以还有一种基于脚本的安装方法比较简单。

# 基于脚本的安装
```access transformers
$ curl -fsSL get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh --mirror Aliyun
```

**安装完成别忘了启动docker**

# 测试是否安装成功
```access transformers
docker run hello-world
```

 如果输出了如下代码则说明安装成功
 
```access transformers
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
ca4f61b1923c: Pull complete
Digest: sha256:be0cd392e45be79ffeffa6b05338b98ebb16c87b255f48e297ec7f98e123905c
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://cloud.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/
```

# 配置镜像管理器 linux系统
  docker的镜像服务器官方的对于大陆用户可能拉取什么的比较慢，所以最好配置一个国内的镜像服务器。
  
1. 对于使用 systemd 的系统，请在 /etc/docker/daemon.json 中写入如下内容（如果文件不存在请新建该文件）对于使用 systemd 的系统，请在 /etc/docker/daemon.json 中写入如下内容（如果文件不存在请新建该文件）
    ```access transformers
       {
         "registry-mirrors": [
           "https://registry.docker-cn.com"
         ]
       }
    ```

2. 重新启动服务
  ```access transformers
    $ sudo systemctl daemon-reload
    $ sudo systemctl restart docker
  ```
 