# Cloud Run

### エンドポイントへのアクセス制御

- `Cloud Run Invoker` から `allusers` を削除する
- 参考：https://shiodaifuku.io/articles/H56ui9aUoKyJ0Dg4oUCe

```bash
# Bearer 認証のIDトークンが必要
$ gcloud auth login
$ gcloud auth print-identity-token
```

- ヘッダに埋め込む
- Chrome では [ModHeader](https://chrome.google.com/webstore/detail/modheader/idgpnmonknjnojddfkpgkljpfnnfcklj) を使える

```
Authorization: Bearer <IDトークン>
```
