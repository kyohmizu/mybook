# Ingress

### GKE で Static IP を設定する

- Static IP を予約

```bash
# 予約
$ gcloud compute addresses create helloweb-ip --global

# 一覧を確認
$ gcloud compute addresses list --global

# 詳細を確認
$ gcloud compute addresses describe helloweb-ip --global
```

- annotation に `kubernetes.io/ingress.global-static-ip-name` を設定

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: helloweb
  annotations:
    kubernetes.io/ingress.global-static-ip-name: helloweb-ip
  labels:
    app: hello
spec:
  backend:
    serviceName: helloweb-backend
    servicePort: 8080
```

- 参考：https://cloud.google.com/kubernetes-engine/docs/tutorials/configuring-domain-name-static-ip

