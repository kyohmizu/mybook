# awk

### 1フィールド目を出力 (print $1)

```bash
echo 1 2 3 4 |awk '{print $1}'
1
```

### $0 はパイプラインから渡された全文字列

```bash
echo 1 2 3 4 |awk '{print $0}'
1 2 3 4

# $0 は省略可
echo 1 2 3 4 |awk '{print}'
1 2 3 4
```


### 参考

[Qiita - awkコマンドの基本](https://qiita.com/yamazon/items/563af1b485ff413d381f)