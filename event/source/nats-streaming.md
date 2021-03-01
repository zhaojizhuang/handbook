## 基于 nats streaming 的 Knative Source

### 1. 发送 example

参考官网 example https://github.com/nats-io/stan.go 
`examples` 下 `stan-pub` 为 `natstreaming` 发送 `demo`

```shell
./stanpub -s nats://10.108.10.16:4222 -c "knative-nats-streaming" -a=true "hello" "hellodata"
```

说明：

- `-s`: 指定 `nats stream` 的地址
- `-c`: 指定 `ClientID`
- `-a`: true 表示异步发送
- `args[0]`: `hello` 表示 `subject`
- `args[1]`: `hellodata` 表示 `message` 的 `data`

为了使用方便 `stan-pub` 和 `stan-sub` 的 二进制已经打到镜像里了

```shell
# 发送消息
[root@k8s-master stan-pub]# docker run -it zhaojizhuang66/nats-cli stanpub -s nats://10.108.10.16:4222 -c "knative-nats-streaming" -a=true "hello" "hellomsg"
# 结果如下：
Published [hello] : 'hellomsg' [guid: 27UkSehtTJ9MesxMtz5c9c]
Received ACK for guid 27UkSehtTJ9MesxMtz5c9c

## 接收消息
[root@k8s-master stan-pub]# docker run -it registry.zjcdn.com/public/oort/faas/nats-cli stansub -s nats://10.108.10.16:4222 -c "knative-nats-streaming" --all=true --new_only=true "hello"
# 结果如下
Connected to nats://10.108.10.16:4222 clusterID: [knative-nats-streaming] clientID: [stan-sub]
Listening on [hello], clientID=[stan-sub], qgroup=[] durable=[]
[#1] Received: sequence:52 subject:"hello" data:"hellodata" timestamp:1614053413688252281
[#2] Received: sequence:53 subject:"hello" data:"hellodata" timestamp:1614053417786333481
```