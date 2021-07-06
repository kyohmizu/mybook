# Tips

### 特定の User や ServiceAccount に紐付いた RoleBinding を一覧表示する

```bash
$ kubectl get rolebinding -n kube-system -o yaml | yq e '.items[] | select(.subjects[] as $i ireduce(false; . or ($i.kind=="User" and $i.name=="system:kube-controller-manager"))) | .metadata.name' -
```

### ロゴや名称のライセンス

https://github.com/cncf/artwork

https://www.linuxfoundation.org/trademark-usage

```
Do not use a logo of The Linux Foundation on posters, brochures, signs, websites, or other marketing materials to promote your events, products or services without written permission from The Linux Foundation.

Do not refer to a product or service as being certified under any of The Linux Foundation’s marks unless your company has successfully undergone the requisite compliance testing and has explicit authorization to use such terms from The Linux Foundation.
```

