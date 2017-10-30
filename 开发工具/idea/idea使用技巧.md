# 使用技巧
## hide Empty Middle Packages
  Project一栏中的小齿轮里面的选项中有这么一个勾选的选项，这个选项是把包名堆积起来显示如果想看更完整
  的目录结构的话，那么就把它勾选掉。
## REST Client
  restClient 这个工具是idea自带的模拟request和post请求，可以用来很好的测试Controller的mapping的url
## 生成serialVersionUID
  serialVersionUID

  这个是用于对象序列化和反序列化的场景，比如我们把一个没有显式定义serialVersionUID的对象序列化到本地，
  然后修改对象添加一个字段，然后进行反序列化，就会发现报错了。
  这是因为serialVersionUID默认会根据对象相关信息来生成一个ID,我们修改了对象相应的他的值就改变了，所以反序列化不会兼容我们的变化，
  所以我们需要显式定义一个serialVersionUID为一个定值，这样我们即使改变了对象也会在反序列化的时候兼容我们的变化。

  idea提供自动生成的功能不过需要进行设置
  1. Serializable class without serialVersionUID 在设置中勾选上
  2. 在类名上按Alt+Enter进行生成