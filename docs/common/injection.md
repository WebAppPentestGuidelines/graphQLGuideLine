# インジェクション系の脆弱性について

## 概要

GraphQLサーバで、受け取ったデータを適切にエスケープまたはエンコードせずに、ほかの処理系に渡した場合、インジェクション攻撃が成立し得ます。

## 影響

インジェクションを受けた処理系に応じてさまざまな影響を受けます。

GraphQLで受け取った入力を渡す先の処理系には、たとえば下記のようなものが想定されます。

- OS
- DBMS(SQL/NoSQL)
- XMLパーサ

このような処理系に信頼できない入力を渡した場合、下記のような攻撃が成立し得ます。

<!-- textlint-disable prh -->
- OSコマンドインジェクション
- SQL/NoSQLインジェクション
- XXE(**X**ML E**x**ternal **E**ntity)
<!-- textlint-enable prh -->

他にも、さまざまなインジェクション系の脆弱性につながる可能性があります。例で挙げたような処理系以外でも、GraphQLサーバが受け取った入力を渡す場合は、適切にその入力値を処理する必要があります。

## 検証方法

実際に、例を見てGraphQLを利用したアプリケーションでSQLインジェクションが成立することを確認しましょう。下記のようなGraphQLのスキーマを想像してください。

```graphql
type Article {
	id: ID
	title: String
	content: String
}

type Query {
	# 記事の情報を取得する
	getArticle(id: ID): Article
}
```

スキーマで定義されたクエリ`getArticle`のリゾルバは下記のように実装されているとします。(サンプルコードは擬似コードです。)

```go
func getArticleResolver(id: string) {
	return query("SELECT * FROM users WHERE id = " + id);
}
```

このとき、ユーザーが下記のようなGraphQLクエリを投げた場合のSQLクエリを考えてみてください。

```graphql
query {
	getArticle(id: "1 UNION SELECT * FROM users") {
		id
		title
		content
	}
}
```

実際に発行されるSQLクエリは、開発者が意図していたものとは異なるものになることがわかります。具体的には、`SELECT * FROM users WHERE id = 1 UNION SELECT * FROM users`というSQLクエリが発行されます。

このように、GraphQLを使用していてもリゾルバで危険なSQLクエリの組み立て方をしていれば、SQLインジェクションの脆弱性につながります。

## 対策

GraphQLサーバが受け取った入力をほかの処理系で利用する場合には、その入力値を必ず適切にエスケープまたはエンコードしてください。また、一般的なインジェクション対策と同様に、入力値のバリデーションも保険的対策として有効です。

各インジェクション系の脆弱性の対策については、ここでの説明は省略します。各インジェクション系の脆弱性の対策について、より詳しい情報が必要であれば下記のようなページをご参照ください。

- [OS Command Injection Prevention](https://cheatsheetseries.owasp.org/cheatsheets/OS_Command_Injection_Defense_Cheat_Sheet.html) [- OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
- [SQL Injection Prevention - OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
- [NoSQL Injection](https://www.netsparker.com/blog/web-security/what-is-nosql-injection/) [- OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
- [XXE Injection Prevention](https://cheatsheetseries.owasp.org/cheatsheets/XML_External_Entity_Prevention_Cheat_Sheet.html) [- OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
    - [XML Security - OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/cheatsheets/XML_Security_Cheat_Sheet.html)