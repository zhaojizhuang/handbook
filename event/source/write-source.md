Knative Source 开发指南
参考 官网  https://knative.dev/docs/eventing/samples/writing-event-source/
准备：
- 安装go-licenses https://github.com/google/go-licenses

1. 相关概念的设计解耦
2. API 定义
3. Controller
4. Reconciler
5. Receive Adapter
6. Example YAML
7. Moving the event source to the knative-sandbox organization

注意：在 MacOS 上运行 hack/update-codegen.sh 时会报错，见 issue https://github.com/knative/hack/issues/50
