## 安装 Knative Eventing

> 此处通过 `operator` 的方式进行安装,此处安装的是 `0.19` 版本

### 1. 安装 operator deploy 和对应的 CRD

参考 https://knative.dev/v0.19-docs/install/knative-with-operators/



```yaml
kubectl apply -f https://github.com/knative/operator/releases/download/v0.19.0/operator.yaml
```



### 2. 通过 CR 安装 KnativeEventing 组件

```yaml
apiVersion: operator.knative.dev/v1alpha1
kind: KnativeEventing
metadata:
  name: knative-eventing
  namespace: knative-eventing
spec:
  version: 0.19.0
  defaultBrokerClass: MTChannelBasedBroker
  config:
    br-default-channel:
      channelTemplateSpec: |
        apiVersion: messaging.knative.dev/v1beta1
        kind: NatssChannel
    br-defaults:
      default-br-config: |
        clusterDefault:
          apiVersion: v1
          brokerClass: MTChannelBasedBroker
          kind: ConfigMap
          name: config-br-default-channel
          namespace: knative-eventing
  registry:
    override:
      eventing-controller/eventing-controller: zhaojizhuang66/eventing/cmd/controller:v0.19.0
      eventing-webhook/eventing-webhook: zhaojizhuang66/eventing/cmd/webhook:v0.19.0
      imc-controller/controller: zhaojizhuang66/eventing/cmd/in_memory/channel_controller:v0.19.0
      imc-dispatcher/dispatcher: zhaojizhuang66/eventing/cmd/in_memory/channel_dispatcher:v0.19.0
      mt-broker-controller/mt-broker-controller: zhaojizhuang66/eventing/cmd/mtchannel_broker:v0.19.0
      mt-broker-filter/filter: zhaojizhuang66/eventing/cmd/mtbroker/filter:v0.19.0
      mt-broker-ingress/ingress: zhaojizhuang66/eventing/cmd/mtbroker/ingress:v0.19.0
      pingsource-mt-adapter/dispatcher: zhaojizhuang66/eventing/cmd/mtping:v0.19.0
      sugar-controller/controller: zhaojizhuang66/eventing/cmd/sugar_controller:v0.19.0






```