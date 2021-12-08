# Tips

### Debug

https://www.terraform.io/docs/internals/debugging.html

```bash
export TF_LOG=DEBUG
export TF_LOG_PATH=./log.txt
```

### Lock の解除

- 参考：https://www.terraform.io/docs/cli/commands/force-unlock.html

```bash
$ terraform force-unlock [options] LOCK_ID [DIR]
```


