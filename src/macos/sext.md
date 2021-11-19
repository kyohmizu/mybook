# System Extensions

https://developer.apple.com/system-extensions/

### Endpoint Security

https://developer.apple.com/documentation/endpointsecurity

- local 環境でのテスト時に必要な設定
  - [Endpoint security system extension misconfiguration](https://developer.apple.com/forums/thread/125048)
  - https://developer.apple.com/documentation/security/disabling_and_enabling_system_integrity_protection
  - https://www.theiphonewiki.com/wiki/AppleMobileFileIntegrity

```bash
# Recovery Mode
csrutil disable

sudo nvram boot-args="amfi_get_out_of_my_way=0x1”
```

- 環境の復旧
  - https://amp.reddit.com/r/OSXTweaks/comments/fvf85o/how_to_revert_amfi_get_out_of_my_way_changes_of/

```bash
# Recovery Mode
csrutil enable

sudo nvram boot-args=""
#sudo defaults write /Library/Preferences/com.apple.security.libraryvalidation.plist DisableLibraryValidation -bool true
```

### 参考

https://knight.sc/reverse%20engineering/2019/08/24/system-extension-internals.html

[Guide for Apple IT: Threat Detection and Apple’s New EndpointSecurity Framework](https://blog.kandji.io/guide-for-apple-it-threat-detection-and-apples-new-endpoint-security-framework)
