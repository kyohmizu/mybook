# nil回避
`to_x` を使うことで `nil` を回避できる。

```ruby
# title に文字列を想定
# これだと nil の場合エラーになる
def capitalize_string(title)
  title.capitalize
end

# to_s を使って nil を '' (空文字)に変換する
# title に文字列が渡ってきたときは self を返すのでオーバーヘッドは最小になっている
def capitalize_string(title)
  title.to_s.capitalize
end

# ボッチ演算子を使うこともできる
def capitalize_string(title)
  title&.capitalize
end
```
