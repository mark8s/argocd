### argocd


#### sync policy
argocd的同步策略

- prune 


默认argocd的同步策略中 `prune: false` ，也就是当你删掉argocd监控的git仓库中的某个文件时， argocd中还会保留对应文化创建的那个资源记录。
当你鼠标移动到那个资源时，会有个提示`OutOfSync（requires pruning）`。






