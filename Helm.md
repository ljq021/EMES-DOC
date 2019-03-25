
### 安装 Helm

可以根据文档安装 helm https://github.com/helm/helm/blob/master/docs/install.md

#### 在 Mac OS 上安装

```shell
# Use homebrew on Mac
brew install kubernetes-helm

# Install Tiller into your Kubernetes cluster
helm init --upgrade -i registry.cn-hangzhou.aliyuncs.com/google_containers/tiller:v2.13.1 --skip-refresh

# update charts repo (Optional)
helm repo update
```

#### 在Windows上安装

```shell
# Use Chocolatey on Windows
# 注：安装的时候需要保证网络能够访问googleapis这个域名
choco install kubernetes-helm

# Install Tiller into your Kubernetes cluster
helm init --upgrade -i registry.cn-hangzhou.aliyuncs.com/google_containers/tiller:v2.13.1 --skip-refresh

# update charts repo (Optional)
helm repo update
```
