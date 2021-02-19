## Broker Trigger
> 介绍 Knative 的 Broker trigger 模型

![](../images/br.png)

### 1. Default Broker

当 `namespace` 或者 `trigger` 携带 `label` `eventing.knative.dev/injection:true` 时，`sugar-controller` 会自动生成对应的 `Broker` 实例

sugar-controller 中包含两个 controller
- namespace controller
- trigger controller

作用：判断 namespace 或 trigger 是否携带 label `eventing.knative.dev/injection:true`，如果有则在同 namespace 下 创建名为 default 的  Broker


### 2. channel 的生成

1. `Broker` 的 创建 会触发 `channel` 的创建  

2. `mt-broker-controller` 会 `watch` `Broker` 的创建，根据 `channeltemplate` 生成对应的 `channel`


### 3. event channel 插件

1. `Natss-ch-controller` 会 `watch` `broker` 生成的 `channel` , 生成 `external service`（`external service` 的地址指向  `natss-ch-dispatcher`）,这个 `external service` 将作为 `natsschannel` 的 `channelAddress`

2. `Natss-ch-controller` 根据 `external service` 更新 `natsschannel` 的 `channelAddress` 

3. `mt-broker-controller` 根据 `natsschannel` 的 `channelAddress` 更新 `broker` `status` 中 的`channelAddress`

