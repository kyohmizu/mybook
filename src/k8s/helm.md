# helm

### リリース削除時に k8s リソースを削除しない

- annotaion に `"helm.sh/resource-policy": keep` を追加する
- https://syu-m-5151.hatenablog.com/entry/2021/06/08/110251

### V2 -> V3

- https://helm.sh/docs/topics/v2_v3_migration/
  - https://github.com/helm/helm-2to3
- https://itnext.io/breaking-changes-in-helm-3-and-how-to-fix-them-39fea23e06ff

```bash
for value in $(helm list --output json | jq '.Releases[] | .Name' | sed 's/\"//g'); do
  helm3 2to3 convert ${value}
done
```

### Helmfile

- HelmfileでKubernetesマニフェストやKustomization、Helm Chartなどで構成されるアプリケーションを効率的に管理する
  - https://thinkit.co.jp/article/18009

### indent or nindent

- `nindent` は改行してからスタートするため、既存スペースの考慮が不要
  - https://banzaicloud.com/blog/creating-helm-charts-part-2/

```yaml
-          resources:
-  {{ toYaml .Values.resources | indent 10 }}
+          resources: {{- toYaml .Values.resources | nindent 12 }}
```

