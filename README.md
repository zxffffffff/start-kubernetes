# start-Kubernetes
 一个 K8s 脚手架，包含各种常用环境

## 简介
参考：https://kubernetes.io/
- K8s 是一个容器编排和管理平台（跨集群自动分发和调度应用容器）
- K8s 也提供常用的开箱即用的微服务相关资源和功能（服务发现、负载均衡、HTTP路由等）


## 1、创建集群
K8s 集群由两种类型的资源组成：
- **控制平面(Control Plane)**协调集群
- **节点(Nodes)**是运行应用程序的工作人员

### 1.1 创建 Minikube 集群
Minikube 用于开发环境，一个节点的简单集群

参考：https://kubernetes.io/zh-cn/docs/tutorials/hello-minikube/

```bash
# macOS 安装
brew install minikube
# 创建集群 (运行在 Docker 中)
minikube start

# 安装插件
minikube addons list # 查看支持的插件
minikube addons enable metrics-server

# 代理访问 Dashboard (默认只能从集群内部网络中访问)
minikube dashboard --url
```

### 1.2 安装 kubectrl 工具
命令行工具，用来部署应用、监测和管理集群资源以及查看日志。

```bash
# macOS 安装
brew install kubectl

# 验证
kubectl cluster-info
```

### 1.3 创建 Deployment
Deployment 是管理 Pod 创建和扩展的推荐方法。

```bash
# 创建管理 Pod 的 Deployment (网络错误可以在 Dashboard 中删除再重试)
kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- /agnhost netexec --http-port=8080

# 查看 Deployment
kubectl get deployments
kubectl get pods
kubectl get events              # 查看集群事件
kubectl config view             # 查看 kubectl 配置
kubectl logs hello-node-xxxxxx  # 查看 Pod 程序日志

# 公开服务 (默认只能从集群内部网络中访问)
kubectl expose deployment hello-node --type=LoadBalancer --port=8080
kubectl get services            # 查看服务
minikube service hello-node
```

### 1.4 停止和清理
```bash
# 清理在集群中创建的资源
kubectl delete service hello-node
kubectl delete deployment hello-node

# 停止 Minikube 集群 (停止在 Docker 中)
minikube stop

# （可选）删除 Minikube VM：
minikube delete
```
