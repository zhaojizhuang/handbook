## Knative Event

### 如何验证 Event

1. 创建 cur 容器，方便发送 event 指令

`kubectl create -f resources/send_example/curl.yaml`

容器 ready 后 登录

`kubectl exec -it curl sh`

```shell
curl -v "http://broker-ingress.faas.svc.cluster.local/default/default" \
  -X POST \
  -H "Ce-Id: say-hello" \
  -H "Ce-Specversion: 1.0" \
  -H "Ce-Type: dev.knative.sources.ping" \
  -H "Ce-Source: not-sendoff" \
  -H "Content-Type: application/json" \
  -d '{"msg":"Hello Knative!"}'
```

其中 `http://broker-ingress.knative-eventing.svc.cluster.local/event-example/default` 为要发送的事件的地址，可以是 `broker` 地址，`filter` 地址，或者 `event receiver` 的地址