# General

```bash
terraform -chdir=.. init -backend-config=./{env}/backend.tfvars -reconfigure
terraform -chdir=.. plan -var-file=./{env}/terraform.tfvars
```
