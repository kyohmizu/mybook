# Secret Store CSI Driver

https://github.com/kubernetes-sigs/secrets-store-csi-driver

install: https://secrets-store-csi-driver.sigs.k8s.io/getting-started/installation.html

### GCP Provider

https://github.com/GoogleCloudPlatform/secrets-store-csi-driver-provider-gcp

- Workload Identity を使用

```bash
export PROJECT_ID=<your gcp project>
gcloud secrets add-iam-policy-binding testsecret \
  --member=serviceAccount:gke-workload@$PROJECT_ID.iam.gserviceaccount.com \
  --role=roles/secretmanager.secretAccessor
```

### Secret リソースの作成について

https://github.com/GoogleCloudPlatform/secrets-store-csi-driver-provider-gcp/issues/37

```bash
# If using the driver to sync secrets-store content as Kubernetes Secrets, deploy the additional RBAC permissions
# required to enable this feature
kubectl apply -f deploy/rbac-secretprovidersyncing.yaml
```

実装サンプル：https://secrets-store-csi-driver.sigs.k8s.io/topics/sync-as-kubernetes-secret.html

- 使用している Pod が消えると Secret も消える
