# Tips

### 特定の User や ServiceAccount に紐付いた RoleBinding を一覧表示する

```bash
$ kubectl get rolebinding -n kube-system -o yaml | yq e '.items[] | select(.subjects[] as $i ireduce(false; . or ($i.kind=="User" and $i.name=="system:kube-controller-manager"))) | .metadata.name' -
```
