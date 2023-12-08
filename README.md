# amqp-proxy

## 介绍
A new AMQP protocol proxy architecture model, based on which Apache Pulsar can better support amqp protocol messages from terminals such as rabbitmq-client, openstack oslo-messaging client.

## 软件架构
软件架构说明


## 安装教程

### 下载Pulsar
下载[Pulsar2.7.2](https://github.com/streamnative/pulsar/releases/download/v2.7.1/apache-pulsar-2.7.1-bin.tar.gz)安装包,并解压

### 下载并构建amqp-proxy插件

下载链接： https://gitee.com/openeuler/amqp-proxy.git

### 从代码中构建

1. 从gitee中下载代码到本地
```bash
git clone https://gitee.com/openeuler/amqp-proxy.git
cd aop
```
2. 编译打包
```bash
mvn clean install -DskipTests
```
3. nar包可以从如下位置获取到
```bash
./amqp-impl/target/pulsar-protocol-handler-amqp-${version}.nar
```

### 配置

|Name|Description|Default|
|---|---|---|
amqpTenant|AMQP on pulsar broker tenant|public
amqpListeners|AMQP service port|amqp://127.0.0.1:5672
amqpMaxNoOfChannels|The maximum number of channels which can exist concurrently on a connection|64
amqpMaxFrameSize|The maximum frame size on a connection|4194304 (4MB)
amqpMaxMessageSize|The maximum message size|4194304 (4MB)

###配置amqp-proxy插件

1. 插件配置
你需要配置“messagingProtocols”和“protocolHandlerDirectory”两个参数，messagingProtocols的值为amqp,protocolHandlerDirectory默认值为“./protocols”，protocolHandlerDirectory配置的是插件位置，运行全需要将nar上传到当前配置的文件夹下

e.g.
```access transformers
messagingProtocols=amqp
protocolHandlerDirectory=./protocols
```

2. 配置AMQP SERVICE LISTENERS
   e.g.
```
amqpListeners=amqp://127.0.0.1:5672
advertisedAddress=127.0.0.1
```

## 使用说明

### 启动服务
以standalone模式启动pulsar服务
```
$PULSAR_HOME/bin/pulsar standalone
```

### 使用rabbitmq客户端进行测试

    ```
    # add RabbitMQ client dependency in your project
    <dependency>
      <groupId>com.rabbitmq</groupId>
      <artifactId>amqp-client</artifactId>
      <version>5.8.0</version>
    </dependency>
    ```

    ```
    // Java Code
    
    // create connection
    ConnectionFactory connectionFactory = new ConnectionFactory();
    connectionFactory.setVirtualHost("vhost1");
    connectionFactory.setHost("127.0.0.1");
    connectionFactory.setPort(5672);
    Connection connection = connectionFactory.newConnection();
    Channel channel = connection.createChannel();
    
    String exchange = "ex";
    String queue = "qu";
    
    // exchage declare
    channel.exchangeDeclare(exchange, BuiltinExchangeType.FANOUT, true, false, false, null);
    
    // queue declare and bind
    channel.queueDeclare(queue, true, false, false, null);
    channel.queueBind(queue, exchange, "");
    
    // publish some messages
    for (int i = 0; i < 100; i++) {
        channel.basicPublish(exchange, "", null, ("hello - " + i).getBytes());
    }
    
    // consume messages
    CountDownLatch countDownLatch = new CountDownLatch(100);
    channel.basicConsume(queue, true, new DefaultConsumer(channel) {
        @Override
        public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
            System.out.println("receive msg: " + new String(body));
            countDownLatch.countDown();
        }
    });
    countDownLatch.await();
    
    // release resource
    channel.close();
    connection.close();
    ```

## 参与贡献

1.  Fork 本仓库
2.  新建 Feat_xxx 分支
3.  提交代码
4.  新建 Pull Request


## 特技

1.  使用 Readme\_XXX.md 来支持不同的语言，例如 Readme\_en.md, Readme\_zh.md
2.  Gitee 官方博客 [blog.gitee.com](https://blog.gitee.com)
3.  你可以 [https://gitee.com/explore](https://gitee.com/explore) 这个地址来了解 Gitee 上的优秀开源项目
4.  [GVP](https://gitee.com/gvp) 全称是 Gitee 最有价值开源项目，是综合评定出的优秀开源项目
5.  Gitee 官方提供的使用手册 [https://gitee.com/help](https://gitee.com/help)
6.  Gitee 封面人物是一档用来展示 Gitee 会员风采的栏目 [https://gitee.com/gitee-stars/](https://gitee.com/gitee-stars/)
