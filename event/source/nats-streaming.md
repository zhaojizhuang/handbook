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