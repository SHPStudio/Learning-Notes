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
- application/json 响应体直接为json格式 
- application/x-www-form-urlencoded 数据本身是json字符串，不过是作为表单的`payload`字段传输

### secret
设置webhook的secret能够让你确保这个POST的请求是从github发出的。具体详情[Securing your webhooks](https://developer.github.com/webhooks/securing/)

### SSL Verification
如果你的`PayloadURL`是一个https的加密网站，你需要设置SSL验证相关的信息。一般只要显示就设置启用就好，会保证发送数据时发送到URL的加密端口(443);

### Active
该webhook传输数据是否是激活的，同样你也可以禁用。

### Events
事件是webhooks的核心，当在代码仓库上执行某个动作时就会触发事件。我们可以选择合适的事件类型，并设为Active，最后完成点击`Add webh`

