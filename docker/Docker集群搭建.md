# 使用Docker Swarm搭建集群
## 时区同步
 首先要做的就是让集群机器时间同步，否则会出现`Error: Error response from daemon: error while validating Root CA Certificate: x509: certificate has expired or is not yet valid`错误。这个是因为两台机器时间不同步问题，加入集群的机器的时间在根证书的开始之前。
### 