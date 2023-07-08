# Controllerにまつわる処理ついて

## Strong Parameters

Strong Parametersとは、Railsで提供されている機能で、フォームから送信されたデータを安全に取得することができます。

[code]

```ruby
private

def create_params
  params.require(:author).permit(
    :name,
    :bio
  )
end
```

- `Parameters: {"authenticity_token"=>"[FILTERED]", "author"=>{"name"=>"Yuki Tanaka", "bio"=>"Yuki Tanaka2"}, "id"=>"326b474f-908b-490d-8d61-9b8d024a677f"}`というようなデータがあるとします。このデータは、`params`というメソッドで取得することができます。

- `params`メソッドは、`ActionController::Parameters`クラスのインスタンスを返します。このクラスは、`Hash`クラスを継承しているので、`Hash`クラスのメソッドを使用することができます。

- `params`メソッドで取得したデータは、`require`メソッドで指定したキーの値を取得することができます。例えば、`params.require(:author)`とすると、`params`メソッドで取得したデータの`:author`というキーの値を取得することができます。

- `permit`メソッドは、`require`メソッドで取得したデータのうち、指定したキーの値を取得することができます。例えば、`params.require(:author).permit(:name, :bio)`とすると、`params`メソッドで取得したデータの`:author`というキーの値のうち、`:name`と`:bio`というキーの値のみ取得することができます。

- ブラウザなどから予期しないデータが送信されたとしても、`permit`メソッドで指定したキーの値のみ取得することができるので、安全にデータを取得することができます。