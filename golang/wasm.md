# WebAssembly

サンプルコード：https://github.com/kyohmizu/go-wasm-sample

### Command

```bash
# copy required files
cp $(go env GOROOT)/misc/wasm/wasm_exec.js .
cp $(go env GOROOT)/misc/wasm/wasm_exec.html .

# build
GOOS=js GOARCH=wasm go build -o main.wasm

# test
GOOS=js GOARCH=wasm go test -exec="$(go env GOROOT)/misc/wasm/go_js_wasm_exec"
```

### 参考

https://zchee.github.io/golang-wiki/WebAssembly/

[Go × WebAssemblyで電卓のWebアプリを作ってみた](https://buildersbox.corp-sansan.com/entry/2019/02/14/113000)

[JSを書かずにGOだけでWEBサイトをつくってみた(WEBASSEMBLY)](https://www.cetus-media.info/article/2021/go-wasm/)
