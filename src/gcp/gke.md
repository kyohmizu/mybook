# GKE

### Admission Webhook

- Control Plane からノードの Admission Webhook へのアクセスは、サービスIPではなくPodIPでアクセスしている。
- Private Cluster の場合はファイアウォールでPodIP許可しないとアクセスできない
  - デフォルトのファイアウォールルールは 10250,443 のみ
- https://github.com/elastic/cloud-on-k8s/issues/1437#issuecomment-516836262
- https://cloud.google.com/kubernetes-engine/docs/how-to/private-clusters#add_firewall_rules


### Workload Identity

- クラスタで有効にする必要がある
- https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity#enable_on_cluster


### metadata server へのアクセス

https://cloud.google.com/compute/docs/storing-retrieving-metadata#querying

https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity#gke_mds

```bash
curl -H "Metadata-Flavor: Google" http://169.254.169.254/computeMetadata/v1/instance/service-accounts/default/email
```
