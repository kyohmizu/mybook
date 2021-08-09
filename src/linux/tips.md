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

### エラーを標準出力に出さないようにする

```bash
$ [command] 2>/dev/null
```

### カラー

https://upload.wikimedia.org/wikipedia/commons/1/15/Xterm_256color_chart.svg

### ポート検索

https://stackoverflow.com/questions/3855127/find-and-kill-process-locking-port-3000-on-mac

```bash
nmap 127.0.0.1

netstat -vanp tcp | grep [PORT]

sudo lsof -i :[PORT]
```

### ビープ音を消す

```bash
# xfce, xsession
xset -b

# terminal
# set bell-style none のコメントアウトを外す
vi /etc/inputrc

sudo modprobe -r pcspkr
# 永続化
echo "blacklist pcspkr" | sudo tee -a /etc/modprobe.d/blacklist
```

### 日本語入力

```bash
apt install ibus-anthy
```

https://askubuntu.com/questions/930493/anthy-japanese-is-using-japanese-keyboard-layout-how-do-i-use-us
