# Tips

### インタラクティブなコマンドを自動化

```bash
$ echo yes | terraform apply
```

### 重複行の削除

- `sort` `uniq` を使用

```bash
$ cat sample.txt | sort | uniq
```

### ファイルの指定行のみ表示

```bash
$ sed -n 10p file
```
