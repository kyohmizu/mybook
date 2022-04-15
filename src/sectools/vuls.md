# Vuls

Created: January 18, 2022 5:00 PM
Edited: March 1, 2022 2:39 PM

## Overview

Agentless Vulnerability Scanner for Linux/FreeBSD

[https://vuls.io/](https://vuls.io/)

## Setup Tool

[https://github.com/vulsio/vulsctl](https://github.com/vulsio/vulsctl)

## Install on Host

[https://vuls.io/docs/en/install-with-vulsctl-host.html](https://vuls.io/docs/en/install-with-vulsctl-host.html)

.bashrc に以下を追加しておく

```bash
export GOROOT=/usr/local/go;
export GOPATH=$HOME/go;
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin;
```

### Scan locally

```bash
$ cat config.toml 
[servers]

[servers.localhost]
host               = "127.0.0.1"
port               = "local"
scanMode           = ["deep"]  # ["short"]

$ ls -l *.sqlite3
-rw-r--r--. 1 kyohei_mizumoto_ex kyohei_mizumoto_ex 1099624448 Jan 18 08:25 cve.sqlite3
-rw-r--r--. 1 kyohei_mizumoto_ex kyohei_mizumoto_ex      65536 Jan 18 08:36 go-exploitdb.sqlite3
-rw-r--r--. 1 kyohei_mizumoto_ex kyohei_mizumoto_ex     167936 Jan 18 08:25 go-kev.sqlite3
-rw-r--r--. 1 kyohei_mizumoto_ex kyohei_mizumoto_ex      40960 Jan 18 08:36 go-msfdb.sqlite3
-rw-r--r--. 1 kyohei_mizumoto_ex kyohei_mizumoto_ex     282624 Jan 18 08:21 gost.sqlite3
-rw-r--r--. 1 kyohei_mizumoto_ex kyohei_mizumoto_ex   16654336 Jan 18 08:20 oval.sqlite3

$ vuls scan
$ vuls report -to-localfile -cvss-over=7.0 -format-csv -diff
$ cat results/current/localhost_short.txt
```

## メモ

- インストール時のユーザが root じゃないと面倒になるかも
    - GOPATH などの関係でスクリプトが正しく動かない
- root でスキャン実行すると、結果ファイルをダウンロードする際にファイルオーナーの変更が必要（めんどくさい）
- DB アップデートのスクリプトは全件更新しようとするので、範囲を限定した方が良い。
    - JVN 2017 は存在しないようで、そこで止まってしまうので初回以降は2018〜2022に限定すると良い
    
    ```bash
    - go-cve-dictionary fetch jvn
    + go-cve-dictionary fetch jvn 2022
    ```
    

## トラブルシューティング

### Failed to scan updatable packages

```bash
Warning: Some warnings occurred.
[Failed to scan updatable packages:
    github.com/future-architect/vuls/scanner.(*redhatBase).scanPackages
        /root/go/src/github.com/future-architect/vuls/scanner/redhatbase.go:292
  - Failed to SSH: execResult: servername: 
      cmd: sudo -S repoquery --all --pkgnarrow=updates --qf='%{NAME} %{EPOCH} %{VERSION} %{RELEASE} %{REPO}'
      exitstatus: 1
      stdout: 
      stderr: sudo: repoquery: command not found
    
      err: exit status 1:
    github.com/future-architect/vuls/scanner.(*redhatBase).scanUpdatablePackages
        /root/go/src/github.com/future-architect/vuls/scanner/redhatbase.go:444 Failed to execute need-restarting:
    github.com/future-architect/vuls/scanner.(*redhatBase).postScan
        /root/go/src/github.com/future-architect/vuls/scanner/redhatbase.go:248
  - Failed to SSH: %!w(scanner.execResult={ {  }   sudo -S LANGUAGE=en_US.UTF-8 needs-restarting  sudo: needs-restarting: command not found
     1 0xc000438f80}):
    github.com/future-architect/vuls/scanner.(*redhatBase).needsRestarting
        /root/go/src/github.com/future-architect/vuls/scanner/redhatbase.go:550]
```

`needs-restarting` がインストールされていないのが原因だった。

以下で解決。

```bash
yum install yum-utils
```

### DB アップデートエラー

```bash
INFO[01-21|08:57:26] Insert 25468 CVEs 
4500 / 25468 [--------------->_______________________________________________________________________] 17.67% ? p/sFailed to insert. dbpath: /home/kyohei_mizumoto_ex/vulsctl/install-host/gost.sqlite3, err: Failed to insert RedHat CVE data. err: Failed to insert. err: too many SQL variables; too many SQL variables; no valid transaction; too many SQL variables; too many SQL variables; no valid transaction; no valid transaction; too many SQL variables; too many SQL variables; no valid transaction; too many SQL variables; too many SQL variables; no valid transaction; no valid transaction; no valid transaction
```

`update-all.sh` の `--batch-size` オプションがエラーになっていた。マシンスペックの問題か？

```bash
/gost.sh --debian --batch-size 500 && \
```

オプションを削除したら解決。（デフォルト値は15）

## GCE (REHL8) への導入手順

```bash
sudo yum install git
sudo yum install yum-utils
git clone https://github.com/vulsio/vulsctl.git

cd vulsctl/install-host/
sudo bash install.sh
vi ~/.bachrc # export GOPATH
./update-all.sh
./cvedb.sh 2018
./cvedb.sh 2019
./cvedb.sh 2020
./cvedb.sh 2021
./cvedb.sh 2022
cp ../config.toml.localscan config.toml
vi config.toml

# Weekly Scan
./update-all.sh
vuls scan
vuls report -to-localfile -cvss-over=7.0 -format-csv -diff
cat results/current/localhost_diff.csv
grep -e 'CVE-ID\|,fixed' results/current/localhost_diff.csv
```

[update-all.sh](http://update-all.sh/)

```bash
#!/bin/sh
  
./oval.sh --redhat && \
#./oval.sh --amazon && \
#./oval.sh --debian && \
#./oval.sh --ubuntu && \
#./oval.sh --alpine && \
#./oval.sh --oracle && \
#./oval.sh --fedora && \
./gost.sh --redhat && \
#./gost.sh --debian && \
#./gost.sh --ubuntu && \
./cvedb.sh 2021 && \
./cvedb.sh 2022 && \
./exploitdb.sh && \
./msfdb.sh
./kev.sh
```

.bashrc

```bash
export GOROOT=/usr/local/go;
export GOPATH=$HOME/go;
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin;
```

config.toml

```bash
[servers]
  
[servers.localhost]
host               = "127.0.0.1"
port               = "local"
scanMode           = ["deep"]
```