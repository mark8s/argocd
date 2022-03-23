### argocd


#### sync policy
argocd的同步策略

- prune

默认argocd的同步策略中 `prune: false` ，也就是当你删掉argocd监控的git仓库中的某个文件时， argocd中还会保留对应文件创建的那个资源记录。
当你鼠标移动到那个资源时，会有个提示`OutOfSync（requires pruning）`。但是当你设置`prune: true`时，argocd在检测你的git中删除了某些资源时，
它也会删除它自己的记录。

- selfHeal

指定是否要回到他们的期望状态恢复资源在集群中修改(默认值:false)。其实就是当你使用cmd修改k8s资源的时候，argocd的状态会变成`OutOfSync`。

如果我们想要将使用cmd修改的k8s资源的状态恢复到git中指定的期望状态，就要设置 `selfHeal: true` 。


- allowEmpty 
  
允许argocd指定git path中的资源文件为空(默认值:false)。当 `allowEmpty: false`时，如果指定git path中的资源文件为空，那么会报错。

错误如：`ComparisonError rpc error: code = Unknown desc = quickstart: app path does not exist`

这时配置`allowEmpty: true` 就ok了。

这里有个坑，路径下的资源为空，但是这个路径必须是要存在的。在git中，如果git某个文件夹下面没有任何文件，那么这个文件夹都会不存在了，所以测试的时候，可以放一个跟k8s无关的文件，保证path存在。

#### 修改healthy判断逻辑
如修改ingress的healthy判断逻辑
```yaml
apiVersion: v1
data:
  resource.customizations.health.networking.k8s.io_Ingress: |
    hs = {}
    hs.status = "Healthy"
    return hs
  resource.customizations.useOpenLibs.networking.k8s.io_Ingress: "true"
kind: ConfigMap
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","kind":"ConfigMap","metadata":{"annotations":{},"labels":{"app.kubernetes.io/name":"argocd-cm","app.kubernetes.io/part-of":"argocd"},"name":"argocd-cm","namespace":"argocd"}}
  creationTimestamp: "2022-02-25T02:46:17Z"
  labels:
    app.kubernetes.io/name: argocd-cm
    app.kubernetes.io/part-of: argocd
  name: argocd-cm
  namespace: argocd
  selfLink: /api/v1/namespaces/argocd/configmaps/argocd-cm
```








