# GraphQLの概要

# GraphQLとは

GraphQLとはFacebookによって開発されたWeb APIの規格です。

RESTful APIにおける課題を解決する技術として注目されており、海外ではGitHub,Twitterなどすでに多くのサービスで採用され、近年では国内でも採用するサービスが増えてきました。

# GraphQLの例

GraphQLはSQLのようなデータを問い合わせるための言語であり、以下2つの言語が存在します。

- クエリ： GraphQL API のリクエストを生成するための言語
- スキーマ： GraphQL API の仕様を記述するための言語

ここでは、ユーザー情報を通知するようなGraphQLサービスを例としてGraphQLの処理を簡単に説明します。

このサービスでは、サーバ側で以下のような内容のスキーマが定義されています。

```graphql
type Query {
    user: User
}
 
type User {
    id: ID
    name: String
    mail: String
}
```

このスキーマではuserというクエリで、Userというオブジェクトを取得することを定義しています。スキーマの記述内容を簡単に説明すると以下となります。

- type Queryはqueryのためにあらかじめ予約されたルート型名であり、フィールドUserを持っています。
- 同様にtype UserはID型であるidフィールド、String型であるnameフィールドとmailフィールドを持っています。

GraphQLでは、クライアント側は処理させたいクエリ指定してリクエストを送信します。クライアント側から以下のようなユーザーの名前を取得するようなクエリが記述されたリクエストを受信したとします。

```graphql
query {
    user {
        name
    }
}
```

サーバはリクエストを受け取ると送信されてきたクエリを上記で説明した定義済みのスキーマに照合して検証し、クエリを実行します。その後、クライアントはレスポンスでjson形式のクエリが処理された結果を受け取ります。

```graphql
{
    "data": {
        "user": {
            "name": "Shindan Taro"
        }
    }
}
```

たとえば名前だけではなくメールアドレスも取得したい場合には以下のようにクエリを変更することで取得できます。

```graphql
query {
    User {
        name
        mail
    }
}
```

```graphql
{
    "data": {
        "User": {
            "name": "Shindan Taro"
            "mail": "taro@example.com"
        }
    }
}
```

また、GraphQLスキーマを以下のように設定することで、クエリに引数を持たせることもできます。
以下の例ではuserConditionという名前のUserConditionオブジェクトにて、ユーザーIDの値を引数にとるGraphQLスキーマを定義しています。

```graphql
type Query {
    user(userCondition: UserCondition!): User
}
 
type User {
    id: ID
    name: String
    mail: String
}

input UserCondition {
    id: ID!
}
```

以下のように引数を伴ったクエリを送信できます。

```graphql
query {
    user(userCondition: { id: "1" }) {
        name
        mail
    }
}
```

このようにクエリを操作することによってクライアント側が欲しい情報の指定が可能であり、一度のリクエストで多くのリソースを取得できることが特徴となっています。
また、多数のエンドポイントが必要となるRESTful APIと違い、1つのエンドポイントで済むという利点もあります。