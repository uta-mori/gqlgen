# gqlgen [![Continuous Integration](https://github.com/99designs/gqlgen/workflows/Continuous%20Integration/badge.svg)](https://github.com/99designs/gqlgen/actions) [![Read the Docs](https://badgen.net/badge/docs/available/green)](http://gqlgen.com/) [![GoDoc](https://godoc.org/github.com/99designs/gqlgen?status.svg)](https://godoc.org/github.com/99designs/gqlgen)

![gqlgen](https://user-images.githubusercontent.com/46195831/89802919-0bb8ef00-db2a-11ea-8ba4-88e7a58b2fd2.png)

## What is gqlgen?

<!-- [gqlgen](https://github.com/99designs/gqlgen) is a Go library for building GraphQL servers without any fuss.<br/>

- **gqlgen is based on a Schema first approach** — You get to Define your API using the GraphQL [Schema Definition Language](http://graphql.org/learn/schema/).
- **gqlgen prioritizes Type safety** — You should never see `map[string]interface{}` here.
- **gqlgen enables Codegen** — We generate the boring bits, so you can focus on building your app quickly.

Still not convinced enough to use **gqlgen**? Compare **gqlgen** with other Go graphql [implementations](https://gqlgen.com/feature-comparison/) -->

[gqlgen](https://github.com/99designs/gqlgen)は、手間をかけずに GraphQL サーバーを構築するための Go ライブラリです。<br/>

- **スキーマファースト** - GraphQL の[スキーマ定義言語](http://graphql.org/learn/schema/)を使って API を定義します。
- **型安全を最優先** - `map[string]interface{}`を見ることはありません。
- **コード生成** - 退屈な部分を生成するので、アプリを素早く構築することに集中することができます。

まだ**gqlgen**を使うことに納得できませんか？**gqlgen**を他の GraphQL の Go[実装](https://gqlgen.com/feature-comparison/)と比較してください。

## Getting Started

<!-- - To install gqlgen run the command `go get github.com/99designs/gqlgen` in your project directory.<br/>
- You could initialize a new project using the recommended folder structure by running this command `go run github.com/99designs/gqlgen init`.

You could find a more comprehensive guide to help you get started [here](https://gqlgen.com/getting-started/).<br/>
We also have a couple of real-world [examples](https://github.com/99designs/gqlgen/tree/master/example) that show how to GraphQL applications with **gqlgen** seamlessly,
You can see these [examples](https://github.com/99designs/gqlgen/tree/master/example) here or visit [godoc](https://godoc.org/github.com/99designs/gqlgen). -->

- `go get github.com/99designs/gqlgen` コマンドをプロジェクトディレクトリで実行することで、gqlgen をインストールできます。<br/>
- `go run github.com/99designs/gqlgen init`コマンドを実行することで、推奨されるフォルダ構造を使用して新しいプロジェクトを初期化できます。

はじめるのに役立つ、より包括的なガイドを[こちら](https://gqlgen.com/getting-started/)で見つけることができます.<br/>。
また、**gqlgen**を使った GraphQL アプリケーションをシームレスに実行する方法を示す、いくつかのリアルワールド[サンプル](https://github.com/99designs/gqlgen/tree/master/example)もあります。
これらの[サンプル](https://github.com/99designs/gqlgen/tree/master/example)は、[godoc](https://godoc.org/github.com/99designs/gqlgen)でも見ることができます。

## Reporting Issues

<!-- If you think you've found a bug, or something isn't behaving the way you think it should, please raise an [issue](https://github.com/99designs/gqlgen/issues) on GitHub. -->

バグを見つけたと思われる場合、または何かが思ったとおりに動作していない場合は、GitHub で[issue](https://github.com/99designs/gqlgen/issues)を提起してください。

## Contributing

<!-- We welcome contributions, Read our [Contribution Guidelines](https://github.com/99designs/gqlgen/blob/master/CONTRIBUTING.md) to learn more about contributing to **gqlgen** -->

貢献を歓迎します。**gqlgen**への貢献の詳細については、[Contribution Guidelines](https://github.com/99designs/gqlgen/blob/master/CONTRIBUTING.md)をお読みください。

<!-- ## Frequently asked questions -->

## よくある質問

<!-- ### How do I prevent fetching child objects that might not be used? -->

### 使わないかもしれない子オブジェクトを取得しないようにするには？

<!-- When you have nested or recursive schema like this:

```graphql
type User {
	id: ID!
	name: String!
	friends: [User!]!
}
```

You need to tell gqlgen that it should only fetch friends if the user requested it. There are two ways to do this;

- #### Using Custom Models

Write a custom model that omits the friends field:

```go
type User struct {
  ID int
  Name string
}
```

And reference the model in `gqlgen.yml`:

```yaml
# gqlgen.yml
models:
  User:
    model: github.com/you/pkg/model.User # go import path to the User struct above
```

- #### Using Explicit Resolvers

If you want to Keep using the generated model, mark the field as requiring a resolver explicitly in `gqlgen.yml` like this:

```yaml
# gqlgen.yml
models:
  User:
    fields:
      friends:
        resolver: true # force a resolver to be generated
```

After doing either of the above and running generate we will need to provide a resolver for friends:

```go
func (r *userResolver) Friends(ctx context.Context, obj *User) ([]*User, error) {
  // select * from user where friendid = obj.ID
  return friends,  nil
}
```

You can also use inline config with directives to achieve the same result

```graphql
directive @goModel(
	model: String
	models: [String!]
) on OBJECT | INPUT_OBJECT | SCALAR | ENUM | INTERFACE | UNION

directive @goField(
	forceResolver: Boolean
	name: String
) on INPUT_FIELD_DEFINITION | FIELD_DEFINITION

type User @goModel(model: "github.com/you/pkg/model.User") {
	id: ID! @goField(name: "todoId")
	friends: [User!]! @goField(forceResolver: true)
}
``` -->

このようなネストされたスキーマまたは再帰的なスキーマがある場合：

```graphql
type User {
	id: ID!
	name: String!
	friends: [User!]!
}
```

gqlgen に、ユーザが要求した場合にのみ friends を取得するように指示する必要があります。これには 2 つの方法があります。

- #### カスタムモデルの使用

friends フィールドを省略したカスタムモデルを作成します：

```go
type User struct {
  ID int
  Name string
}
```

そして、`gqlgen.yml`でそのモデルを参照してください：

```yaml
# gqlgen.yml
models:
  User:
    model: github.com/you/pkg/model.User # 上記のUser構造体へのパス
```

- #### 明示的なリゾルバーの使用

生成されたモデルを使い続けたい場合は、次のように `gqlgen.yml` で明示的にリゾルバを必要とすること指示してください：

```yaml
# gqlgen.yml
models:
  User:
    fields:
      friends:
        resolver: true # 強制的にリゾルバを生成する
```

上記のいずれかを実行して generate を実行した後、friends 用のリゾルバを提供する必要があります：

```go
func (r *userResolver) Friends(ctx context.Context, obj *User) ([]*User, error) {
  // select * from user where friendid = obj.ID
  return friends,  nil
}
```

同じ結果を得るために、directives を使ったインライン設定を使うこともできます。

```graphql
directive @goModel(
	model: String
	models: [String!]
) on OBJECT | INPUT_OBJECT | SCALAR | ENUM | INTERFACE | UNION

directive @goField(
	forceResolver: Boolean
	name: String
) on INPUT_FIELD_DEFINITION | FIELD_DEFINITION

type User @goModel(model: "github.com/you/pkg/model.User") {
	id: ID! @goField(name: "todoId")
	friends: [User!]! @goField(forceResolver: true)
}
```

<!-- ### Can I change the type of the ID from type String to Type Int? -->

### ID の型を String 型から Int 型に変更できますか？

<!-- Yes! You can by remapping it in config as seen below:

```yaml
models:
  ID: # The GraphQL type ID is backed by
    model:
      - github.com/99designs/gqlgen/graphql.IntID # An go integer
      - github.com/99designs/gqlgen/graphql.ID # or a go string
```

This means gqlgen will be able to automatically bind to strings or ints for models you have written yourself, but the
first model in this list is used as the default type and it will always be used when:

- Generating models based on schema
- As arguments in resolvers

There isn't any way around this, gqlgen has no way to know what you want in a given context. -->

はい！以下のように設定でリマップすることで可能です：

```yaml
models:
  ID: #  GraphQLの型IDは
    model:
      - github.com/99designs/gqlgen/graphql.IntID # integer型
      - github.com/99designs/gqlgen/graphql.ID # またはstring型
```

これは、gqlgen が自分で書いたモデルに対して自動的に String や Int にバインドできることを意味しますが、このリストの最初のモデルはデフォルトの型として使用され、常にその型が使用されます：

- スキーマに基づいてモデルを生成する
- リゾルバの引数として

これを回避する方法はありません、gqlgen は与えられたコンテキストで何が欲しいかを知る方法を持っていません。

## Other Resources

- [Christopher Biscardi @ Gophercon UK 2018](https://youtu.be/FdURVezcdcw)
- [Introducing gqlgen: a GraphQL Server Generator for Go](https://99designs.com.au/blog/engineering/gqlgen-a-graphql-server-generator-for-go/)
- [Dive into GraphQL by Iván Corrales Solera](https://medium.com/@ivan.corrales.solera/dive-into-graphql-9bfedf22e1a)
- [Sample Project built on gqlgen with Postgres by Oleg Shalygin](https://github.com/oshalygin/gqlgen-pg-todo-example)
