# netty
 netty是一个java网络编程框架，封装了底层网络传输的复杂代码，对外提供非常简易的api，我们可以使用这些api去构建一个网络客户端和服务端。
## netty与tomcat的区别
  最大的区别就是他们的通讯协议了，tomcat是web服务器，是基于http协议进行网络传输的。
  
  然而netty是可以自己定义协议，通过codec来编码/解码字节流来进行传输。
## netty的优点
1. 传输快
2. 封装好
3. 并发高

### 并发高
  并发高是因为netty是使用了NIO(非阻塞IO)的通信框架，相对于传统的BIO(阻塞IO)并发性能提高了很多。
  
1. bio  bio是每一个io请求都占用一个线程，读和写都会阻塞这个线程。
2. nio nio通过一个selector调度模块，来处理读和写的请求，当请求socket完全建立后才通知thread去处理。一个selector对应一个线程，所以在单线程情况下nio性能会比bio要高。

### 传输快
 一般我们从io读取数据的时候，数据需要先经过socket缓冲区，然后再从缓冲区再到我们java的堆内存，也就是说数据传输经历了两次拷贝。
 
 nio有零拷贝的特性，他是直接在堆内存开辟了一块内存，从io读取的数据直接就存到了这块内存。
 
 然后通过ByteBuf这个容器去操作这些数据。

### 封装好
 netty把传统的socket程序的底层实现进行封装，封装后形成了非常简洁的api，在代码量上秒杀传统socket编程模式。
 
 1. channel
 
   每一个channel是一个连接也就是请求，这个连接通过channelPipeline管道，并经过管道里一些channelHandler的处理，最后把处理后的数据进行输出，
   Handler之间的数据传输是通过ChannelHandlerContext来进行业务数据传输的。
 2. ByteBuf
   他是一个存储字节的容器，方便的对字节进行一些getset操作，并支持整段字节索引等等方便的技术。
 3. codec
   他是一个基于字节的编码解码器，可以实现把pojo与我们的字节进行相互的转化。 