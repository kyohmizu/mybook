# CVE

### Data Source

https://nvd.nist.gov/vuln

https://cve.mitre.org/cve/

https://www.securityfocus.com/

### CVE とは

[IPA - 共通脆弱性識別子CVE概説](https://www.ipa.go.jp/security/vuln/CVE.html)

### CWE とは

[IPA - 共通脆弱性タイプ一覧CWE概説](https://www.ipa.go.jp/security/vuln/CWE.html)

### 脆弱性調査ログ

```
# インストールされているか一覧でチェック
yum list installed
```

- Amazon Linux Security Advisory
  - https://alas.aws.amazon.com/alas2.html
- ISC DHCP
  - [ISC's Software Support Policy and Version Numbering](https://kb.isc.org/docs/aa-00896)
  - https://www.brennan.id.au/10-DHCP_Server.html
- curl
  - https://stackoverflow.com/questions/4976971/compiling-php-with-curl-where-is-curl-installed
- named (BIND)
  - /etc/named.conf
- glibc
  - [【Linux】glibcのバージョンを確認する方法](https://www.softel.co.jp/blogs/tech/archives/5282)
- Glib
  - https://gitlab.gnome.org/GNOME/glib/-/blob/main/INSTALL.in
- libxml2
  - https://www.unix.com/unix-for-dummies-questions-and-answers/150326-version-libxml2.html
  - https://stackoverflow.com/questions/48756209/how-are-you-supposed-to-know-what-library-names-are
- nss
  - https://access.redhat.com/solutions/1227863
- python
  - [パッチ情報](https://www.python.org/downloads/)
- systemd
  - https://superuser.com/questions/1282434/how-to-find-out-the-systemd-version-on-raspbian

### CPE

https://nvd.nist.gov/products

https://www.ipa.go.jp/security/vuln/CPE.html

```txt
# CPE 2.2
cpe:/a:kubernetes:kubernetes

# CPE 2.3
cpe:2.3:a:kubernetes:kubernetes:*:*:*:*:*:*:*:*
```

### Tips

- Candidate(候補) ステータスは廃止されたらしい
- Phase は今では常に Assigned になる

