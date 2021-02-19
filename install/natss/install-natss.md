## Knative 对接 nats streaming

### 1. Nats Streaming 安装

下载代码 https://github.com/knative-sandbox/eventing-natss.git  
此处下载的 `release-0.20` 的代码(`release-0.19`有个未修复的 bug)

```shell
kubectl create namespace natss
kubectl apply -n natss -f ./config/broker/natss.yaml
```
参考官网 [install readme](https://github.com/knative-sandbox/eventing-natss/blob/release-0.20/config/broker/README.md)

### 2. eventing-natss 安装

#### 提前准备好 pv

下面列出来一个 `local pv` 的例子，注意 `path` 要提前创建好  `/mnt/disks/ssd2`，`mod` 最好改为 `777`
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: task-pv-volume2
  labels:
    type: local
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/disks/ssd2"
```

#### 安装 eventing-natss


```shell
wget https://github.com/knative-sandbox/eventing-natss/releases/download/v0.19.0/eventing-natss.yaml
kubectl apply -f  eventing-natss.yaml
```


#### Natss Broker 
当前 Natss 没有实现自己的 broker 逻辑，代码处于 beta 版本，当前只实现了 channel，broker 暂未公开 release。通过 e2e 的代码可以看到， natss Broker 的 ingress 暂时用的 MTChannel的broker 入口
代码地址 在 https://github.com/knative-sandbox/eventing-natss/tree/master/test/e2e/config/direct


e2e 中代码如下,此处仅供参考，通过之前  [安装 Knative Eventing](../knative/knativeeventing.md) 中 `operator` 安装之后 自动就配置了 `natss streaming` 为默认的 `channel` 

```yaml
Broker 
apiVersion: eventing.knative.dev/v1
kind: Broker
metadata:
  name: testbroker
  namespace: {{ .namespace }}
  annotations:
    eventing.knative.dev/broker.class: MTChannelBasedBroker
spec:
  config:
    apiVersion: v1
    kind: ConfigMap
    name: config-natss-channel
    namespace: natss

Trigger
apiVersion: eventing.knative.dev/v1
kind: Trigger
metadata:
  name: testtrigger
  namespace: {{ .namespace }}
spec:
  broker: testbroker
  subscriber:
    ref:
      apiVersion: v1
      kind: Service
      name: recorder

config-natss-channel
apiVersion: v1
kind: ConfigMap
metadata:
  name: config-natss-channel
  namespace: natss
  labels:
    eventing.knative.dev/release: devel
data:
  channelTemplateSpec: |
    apiVersion: messaging.knative.dev/v1beta1
    kind: NatssChannel
```
