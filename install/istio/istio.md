## istio 安装

> istio 也是通过 operator 来安装

### 1. 安装 istio 的operator 和crd

 通过 istioctl 生成对应的yaml ,其中 `--hub docker.io/istio`可以换成自己的私有仓库地址，docker.io 为默认生成的

```yaml
istioctl operator  dump --hub docker.io/istio > istio_operator.yaml
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
      hub: docker.io/istio  # 此处可以改为自己的私有仓库
      tag: 1.8.1
      imagePullPolicy: IfNotPresent
      proxy:
        clusterDomain: cluster.local
      trustDomain: cluster.local
```

