## 安装 Knative Serving

> 此处通过 `operator` 的方式进行安装,此处安装的是 `0.19` 版本

### 1. 安装 operator deploy 和对应的 CRD

参考 https://knative.dev/v0.19-docs/install/knative-with-operators/



```yaml
kubectl apply -f https://github.com/knative/operator/releases/download/v0.19.0/operator.yaml
```



### 2. 通过 CR 安装 KnativeServing 组件

```yaml
apiVersion: operator.knative.dev/v1alpha1
kind: KnativeServing
metadata:
  name: knative-serving
  namespace: knative-serving
spec:
  version: 0.19.0
  # config
  config:
    defaults:
      container-concurrency: "50"
    autoscaler:
      enable-scale-to-zero: "true"
    deployment:
      # replace queue
      queueSidecarImage: zhaojizhuang66/serving/cmd/queue:v0.19.0
    domain:
      faas.oortgslb.com: |
    network:
      domainTemplate: |-
        {{if index .Annotations "my.func/subdomain" -}}
          {{- index .Annotations "my.func/subdomain" -}}
        {{else -}}
          {{- .Name}}.{{.Namespace -}}
        {{end -}}
          .{{.Domain}}
    istio:
      local-gateway.knative-serving.cluster-local-gateway: "cluster-local-gateway.istio-system.svc.cluster.local"
  # image
  registry:
    override:
      # replace images 此处可换为自己的私有镜像
      activator: gcr.io/knative-releases/knative.dev/serving/cmd/activator:v0.19.0
      webhook: gcr.io/knative-releases/knative.dev/serving/cmd/webhook:v0.19.0
      controller: gcr.io/knative-releases/knative.dev/serving/cmd/controller:v0.19.0
      autoscaler: gcr.io/knative-releases/knative.dev/serving/cmd/autoscaler:v0.19.0
      autoscaler-hpa: gcr.io/knative-releases/knative.dev/serving/cmd/autoscaler-hpa:v0.19.0
      istio-webhook/webhook: gcr.io/knative-releases/knative.dev/net-istio/cmd/webhook:v0.19.0
      networking-istio: gcr.io/knative-releases/knative.dev/net-istio/cmd/controller:v0.19.0
      migrate: zhaojizhuang66/migrate:v0.19.0
```