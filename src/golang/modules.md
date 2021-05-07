# go modules

### 初期化

- `go mod init` を使用
- 参考：https://qiita.com/uchiko/items/64fb3020dd64cf211d4e

```bash
$ mkdir go-mod-sample
$ cd go-mod-sample
$ go mod init example.com/go-mod-sample

$ cat go.mod
module example.com/go-mod-sample

go 1.16
```