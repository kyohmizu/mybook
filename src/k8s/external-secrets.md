# External Secrets

https://github.com/external-secrets/kubernetes-external-secrets

### Install

```bash
$ helm repo add external-secrets https://external-secrets.github.io/kubernetes-external-secrets/
$ helm install [RELEASE_NAME] external-secrets/kubernetes-external-secrets
```

### General

- Secret Manager の情報を読んで、クラスタ内に Secrets を作成する
- Secret Manager の読み込み権限が必要
  - GCP は WorkloadIdentity が必要

