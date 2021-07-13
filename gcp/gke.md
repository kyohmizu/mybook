# GKE

### Admission Webhook

- Control Plane からノードの Admission Webhook へのアクセスは、サービスIPではなくPodIPでアクセスしている。
- Private Cluster の場合はファイアウォールでPodIP許可しないとアクセスできない
  - デフォルトのファイアウォールルールは 10250,443 のみ
- https://github.com/elastic/cloud-on-k8s/issues/1437#issuecomment-516836262
- https://cloud.google.com/kubernetes-engine/docs/how-to/private-clusters#add_firewall_rules

