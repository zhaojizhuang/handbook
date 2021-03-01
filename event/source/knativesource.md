## Knative Source 的原理

### 1. Event 发送者

先看下面这个例子

[heartbeats](https://github.com/knative/eventing-contrib/blob/master/cmd/heartbeats/main.go)

应用首先获取 `K_SINK` 和 `K_CE_OVERRIDES` 两个环境变量，`K_SINK` 决定发送的目的地