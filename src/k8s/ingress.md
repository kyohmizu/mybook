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

### GKE で HTTPS 接続に事前に用意した証明書を使用する

- 事前に証明書を作成しておく
  - 参考：[Google マネージド SSL 証明書の使用](https://cloud.google.com/load-balancing/docs/ssl-certificates/google-managed-certs)
- ingress リソースに `kubernetes.io/pre-shared-cert` annotation を付与する
  - 参考：[GKE - Google マネージド SSL 証明書の使用](https://cloud.google.com/kubernetes-engine/docs/how-to/managed-certs)

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: helloweb
  annotations:
    ingress.gcp.kubernetes.io/pre-shared-cert: helloweb-cert
  labels:
    app: hello
spec:
  backend:
    serviceName: helloweb-backend
    servicePort: 8080
```


