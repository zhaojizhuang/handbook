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
```