# sops terraform provider

## 概要

sops を使用して、Secret を暗号化した状態で terraform 管理する

キーには AWS や GCP の KMS を使用できる

## sops の使い方

https://github.com/mozilla/sops

インストール

```bash
brew install sops
```

キー作成

```bash
# GCPサンプル
gcloud auth login
gcloud auth application-default login
#export LOCATION=[ex. us-west2]
#export PROJECT=
gcloud kms keyrings create sops --location ${LOCATION} --project ${PROJECT}
gcloud kms keys create sops-key --location ${LOCATION} --project ${PROJECT} --keyring sops --purpose encryption
gcloud kms keys list --location ${LOCATION} --keyring sops --project ${PROJECT}
```

暗号化、複合化（GCP）

raw, json, yaml 形式に対応。json や yaml は各 value のみ暗号化する

```bash
# GCPサンプル
# Cloud KMS CryptoKey Encrypter/Decrypter の権限が必要
sops --encrypt --gcp-kms projects/my-project/locations/global/keyRings/sops/cryptoKeys/sops-key test.yaml > test.enc.yaml
sops --decrypt test.enc.yaml

# サンプル
for value in secret1.txt secret2.txt secret3.txt; do
  sops --encrypt --gcp-kms projects/${PROJECT}/locations/${LOCATION}/keyRings/sops/cryptoKeys/sops-key data/${value} > sops/${value}
done
```

## terraform

https://github.com/carlpett/terraform-provider-sops

GCPサンプル

```
terraform {
  required_providers {
    google = {
      source  = "hashicorp/google"
      version = "3.83.0"
    }
    sops = {
      source  = "carlpett/sops"
      version = "~> 0.5"
    }
  }
}

data "sops_file" "secret" {
  source_file = "sops/secret.txt"
  input_type  = "raw"
}

resource "google_secret_manager_secret_version" "secret" {
  secret      = google_secret_manager_secret.secret.id
  secret_data = data.sops_file.secret.raw

  lifecycle {
    prevent_destroy = true
  }
}
```

## 注意事項等メモ

- 複合化された Secret が tfstate に乗ってしまう
    - tfstate へのアクセス制御に気をつける（GCS は暗号化できないので特に）
    - とはいえ pod にマウントされたら普通に見れてしまうし、そこまで気にすることでもないか...？
- yaml 形式で暗号化、複合化すると yaml のインデントがスペース4つになる
    - terraform import した時に差分が出て困った
    - raw ファイルとして暗号化することで対応（拡張子を .yaml → .txt にした）
- secret_data は sensitive value なので、通常の plan では表示されない
    - [terraform plan で sensitive values を参照する](https://www.notion.so/terraform-plan-sensitive-values-e01c8a27e99145b8b3afe9a6cf982ce3)

## 参考

https://github.com/mozilla/sops

https://github.com/carlpett/terraform-provider-sops

[Terraform の秘匿情報を mozilla/sops で管理する - chroju.dev/blog](https://chroju.dev/blog/terraform_with_sops)