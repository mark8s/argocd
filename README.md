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










