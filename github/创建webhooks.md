# 创建webhooks
  创建webhooks大概分两步
  1. 首先需要设置要监听哪种类型的事件
  2. 然后设置你要接收事件信息的服务器和管理发送的事件数据。

## 设置webhook
设置一个webhook有两种方式
1. 去你的代码仓库或者组织的设置页面(settings),然后点击`WebHooks`->`Add webhook`
2. 使用github提供的[Webhooks API](https://developer.github.com/v3/repos/hooks/)来构建和管理webhook

webhook在使用前需要一些配置:

### PayloadURL
PayloadURL是接收webhook Post请求的服务器地址。

由于我们可能没有一个专门的服务器用来测试是否设置成功，不过github给了一个用来测试的url`http://localhost:4567/payload`,详情请看[Configuring Your Server](https://developer.github.com/webhooks/configuring/)

### Content Type
webhook数据传输格式支持两种
- application/json 响应体是json格式 
- application/x-www-form-urlencoded 数据本来
