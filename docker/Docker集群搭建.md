# 使用Docker Swarm搭建集群
## 时区同步
 首先要做的就是让集群机器时间同步，否则会出现`Error: Error response from daemon: error while validating Root CA Certificate: x509: certificate has expired or is not yet valid`错误。这个是因为两台机器时间不同步问题，加入集群的机器的时间在根证书的开始之前。

### 同步时间
  通过命令`timedatectl set-timezone Asia/Shanghai`和`timedatectl set-ntp on`调整时区，让时间同步

## 构建集群
 首先在集群中的一个节点构建manager节点。使用`docker swarm init --advertise-addr <ip>`来创建，创建成功会提示通过一段带有token的命令来使**工作节点***加入