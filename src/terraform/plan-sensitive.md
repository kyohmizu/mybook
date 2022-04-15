# terraform plan で sensitive values を参照する

## 動機

sops を使って Secret Manager Secret を terraform 管理にした際に、secret_data の差分が出て replace となっていたのでデバッグしたかった

[sops - terraform provider](https://www.notion.so/sops-terraform-provider-2bdda265e4f34341af569d483caedf1f) 

普通に plan するとこうなる

```
# module.bkp.google_secret_manager_secret_version.secret must be replaced
-/+ resource "google_secret_manager_secret_version" "secret" {
      ~ create_time  = "2021-10-28T10:00:42.658474Z" -> (known after apply)
      ~ destroy_time = "" -> (known after apply)
      ~ id           = "projects/bitkey-platform-edge/secrets/secret/versions/1" -> (known after apply)
      ~ name         = "projects/826097459092/secrets/secret/versions/1" -> (known after apply)
      # Warning: this attribute value will no longer be marked as sensitive
      # after applying this change.
      ~ secret_data  = (sensitive value) # forces replacement
        # (2 unchanged attributes hidden)

      - timeouts {}
    }

Plan: 1 to add, 0 to change, 1 to destroy.
```

## 参照方法

terraform plan の結果をファイルに出力し、terraform show -json で表示すると見れる

```
terraform plan -target=module.bkp.google_secret_manager_secret_version.secret -out=tfplan
terraform show -json tfplan | jq | tee tfplan.json
```

before, after でそれぞれ表示される

```
"before": {
          "create_time": "2021-10-28T10:00:42.658474Z",
          "destroy_time": "",
          "enabled": true,
          "id": "projects/bitkey-platform-edge/secrets/secret/versions/1",
          "name": "projects/826097459092/secrets/secret/versions/1",
          "secret": "projects/bitkey-platform-edge/secrets/secret",
          "secret_data": "raw_message: xxxxxxxxxxxxxxxxx",
          "timeouts": {
            "create": null,
            "delete": null
          }
        },
        "after": {
          "create_time": "2021-10-28T10:00:42.658474Z",
          "destroy_time": "",
          "enabled": true,
          "id": "projects/bitkey-platform-edge/secrets/secret/versions/1",
          "name": "projects/826097459092/secrets/secret/versions/1",
          "secret": "projects/bitkey-platform-edge/secrets/secret",
          "secret_data": "raw_message: xxxxxxxxxxxxxxxxxx",
          "timeouts": {
            "create": null,
            "delete": null
          }
        },
```

## 参考

[How to show sensitive values](https://discuss.hashicorp.com/t/how-to-show-sensitive-values/24076)
