# amqp-proxy

## Description
A new AMQP protocol proxy architecture model, based on which Apache Pulsar can better support amqp protocol messages from terminals such as rabbitmq-client, openstack oslo-messaging client.

## Software Architecture
Software architecture description

## Installation


### Download Pulsar
Download [Pulsar 2.7.2](https://github.com/streamnative/pulsar/releases/download/v2.7.2/apache-pulsar-2.7.2-bin.tar.gz) binary package `apache-pulsar-2.7.2-bin.tar.gz`. and unzip it.

### Download and Build amqp-proxy Plugin
download url: https://gitee.com/openeuler/amqp-proxy.git

### build from code
1. Clone the project from gitee to your local.
```bash
git clone https://gitee.com/openeuler/amqp-proxy.git
cd aop
```

2. Compile and package
```bash
mvn clean install -DskipTests
```

3. You can find the nar file in the following directory.
```bash
./amqp-impl/target/pulsar-protocol-handler-amqp-${version}.nar
```

### Configuration

|Name|Description|Default|
|---|---|---|
amqpTenant|AMQP on pulsar broker tenant|public
amqpListeners|AMQP service port|amqp://127.0.0.1:5672
amqpMaxNoOfChannels|The maximum number of channels which can exist concurrently on a connection|64
amqpMaxFrameSize|The maximum frame size on a connection|4194304 (4MB)
amqpMaxMessageSize|The maximum message size|4194304 (4MB)

### Configure amqp-proxy plugin
1. Protocol handler configuration

You need to add “messagingProtocols”和“protocolHandlerDirectory”，For AoP, the value for `messagingProtocols` is `amqp`; the value for `protocolHandlerDirectory` is the directory of AoP nar file.

e.g.
```access transformers
messagingProtocols=amqp
protocolHandlerDirectory=./protocols
```
2. Set AMQP service listeners

Set AMQP service `listeners`. Note that the hostname value in listeners is the same as Pulsar broker's `advertisedAddress`.

The following is an example.
```
amqpListeners=amqp://127.0.0.1:5672
advertisedAddress=127.0.0.1
```


## Instructions

1.  xxxx
2.  xxxx
3.  xxxx

## Contribution

1.  Fork the repository
2.  Create Feat_xxx branch
3.  Commit your code
4.  Create Pull Request


## Gitee Feature

1.  You can use Readme\_XXX.md to support different languages, such as Readme\_en.md, Readme\_zh.md
2.  Gitee blog [blog.gitee.com](https://blog.gitee.com)
3.  Explore open source project [https://gitee.com/explore](https://gitee.com/explore)
4.  The most valuable open source project [GVP](https://gitee.com/gvp)
5.  The manual of Gitee [https://gitee.com/help](https://gitee.com/help)
6.  The most popular members  [https://gitee.com/gitee-stars/](https://gitee.com/gitee-stars/)
