
### 配置 Istio

说明：Istio Ingress Gateway和Ingress缺省的端口冲突，请移除Ingress并进行下面测试

可以根据文档安装 Istio https://istio.io/docs/setup/kubernetes/

#### 下载 Istio 1.0.3 并安装 CLI

```bash
curl -L https://git.io/getLatestIstio | sh -
cd istio-1.0.3/
export PATH=$PWD/bin:$PATH
```

在Windows上，您可以手工下载Istio安装包，或者把```getLatestIstio.ps1```拷贝到你希望下载 Istio 的目录，并执行 - 说明：根据社区提供的[安装脚本](https://gist.github.com/kameshsampath/796060a806da15b39aa9569c8f8e6bcf)修改而来

```powershell
.\getLatestIstio.ps1
```



#### 通过 Helm chart 安装 Istio

```shell
helm install install/kubernetes/helm/istio --name istio --namespace istio-system
```

#### 查看 istio 发布状态

```shell
helm status istio
```

#### 为 ```default``` 名空间开启自动 sidecar 注入

```shell
kubectl label namespace default istio-injection=enabled
kubectl get namespace -L istio-injection
```

#### 安装 Book Info 示例

```shell
kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
```


确认示例应用在运行中

```bash
export GATEWAY_URL=localhost:80
curl -o /dev/null -s -w "%{http_code}\n" http://${GATEWAY_URL}/productpage
```

可以通过浏览器访问

http://localhost/productpage


说明：如果当前80端口已经被占用或保留，我们可以编辑 ```install/kubernetes/helm/istio/values.yaml``` 文件中
Gateway 端口进行调整，比如将 80 端口替换为 8888 端口

```
      ## You can add custom gateway ports
    - port: 8888  # Changed from 80
      targetPort: 80
      name: http2
      nodePort: 31380
```

然后执行如下命令并生效

```shell
kubectl delete service istio-ingressgateway -n istio-system
helm upgrade istio install/kubernetes/helm/istio
```

#### 删除实例应用

```bash
samples/bookinfo/platform/kube/cleanup.sh
```

### 卸载 Istio

```shell
helm del --purge istio
kubectl delete -f install/kubernetes/helm/istio/templates/crds.yaml -n istio-system
```

