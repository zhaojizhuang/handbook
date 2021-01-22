## istio 安装

> istio 也是通过 operator 来安装

### 1. 安装 istio 的operator 和crd

 通过 istioctl 生成对应的yaml 

```yaml
istioctl operator  dump --hub zhaojizhuang66/istio > istio_operator.yaml
```

```yaml
kubectl apply -f istio_operator.yaml # 安装 operator 和 CR 资源
```



### 2. 安装对应的 CR 资源



```yaml
apiVersion: install.istio.io/v1alpha1
kind: IstioOperator
metadata:
  namespace: istio-system
  name: istiocontrolplane
spec:
  components:
    ingressGateways:
      - enabled: true
        name: istio-ingressgateway
      - enabled: true
        label:
          app: cluster-local-gateway
          istio: cluster-local-gateway
        name: cluster-local-gateway
  meshConfig:
    rootNamespace: istio-system
  values:
    global:
      istioNamespace: istio-system
      hub: zhaojizhang66/istio
      tag: 1.8.1
      imagePullPolicy: IfNotPresent
      proxy:
        clusterDomain: svc.cluster.local
      trustDomain: svc.cluster.local
```

