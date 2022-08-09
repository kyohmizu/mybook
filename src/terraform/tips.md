# Tips

## Debug

https://www.terraform.io/docs/internals/debugging.html

```bash
export TF_LOG=DEBUG
export TF_LOG_PATH=./log.txt
```

## plan の詳細

```bash
terraform plan -out=tfplan
terraform show -json tfplan
```

## Lock の解除

- 参考：https://www.terraform.io/docs/cli/commands/force-unlock.html

```bash
$ terraform force-unlock [options] LOCK_ID [DIR]
```


