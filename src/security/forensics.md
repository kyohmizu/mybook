# Forensics

https://github.com/mikeroyal/Digital-Forensics-Guide

### Maliware Analysis

https://yara-ci.cloud.virustotal.com/

https://github.com/mandiant/stringsifter

https://github.com/JPCERTCC/jpcert-yara

### Cloud

https://github.com/google/cloud-forensics-utils

### Windows

[小規模CSIRT向けWindowsイベントログで押さえておくこと](https://ierae.co.jp/blog/csirt_windows_event_log/)

https://ghidra-sre.org/

https://malapi.io/

https://github.com/Johnng007/Live-Forensicator

https://github.com/Yamato-Security/hayabusa

### macOS

[Workshop: An Introduction to macOS Forensics with Open Source Software](https://jsac.jpcert.or.jp/archive/2022/pdf/JSAC2022_workshop_macOS-forensic_jp.pdf)

```bash
# sample commands
$ sudo ./ProcessMonitor.app/Contents/MacOS/ProcessMonitor > mina_procmon.json
$ sudo ./FileMonitor.app/Contents/MacOS/FileMonitor > mina_filemon.json
$ chmod +x _mina
$ ./_mina

$ strings -a ./exported_files/_mina | grep com.aex-loop.agent.plist
$ jq '. | select(.process.name == "_mina" and (.event | endswith("EXEC")))' json/mina_procmon.json 2>/dev/null
$ jq '. | select(.process.ppid == 803 and (.event | (endswith("EXEC") or endswith("FORK"))))' json/mina_procmon.json 2>/dev/null
$ jq '. | select(.process.pid == 805 and (.event | (endswith("EXEC") or endswith("FORK"))))' json/mina_procmon.json 2>/dev/null
$ jq '. | select((.file.process.name == "_mina") and (.event | endswith("CREATE")))' json/mina_filemon.json 2>/dev/null
```

https://github.com/ydkhatri/mac_apt

https://github.com/mnrkbys/ma2tl

https://sqlitebrowser.org/

https://themittenmac.com/the-truetree-concept/

https://objective-see.com/products/netiquette.html

https://github.com/mnrkbys/macosac

https://objective-see.com/products/knockknock.html
