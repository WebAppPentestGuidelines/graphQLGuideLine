# Introspection Query

## 概要

Introspectionとは、GraphQLがどのようなスキーマ情報をサポートしているのかを問い合わせるための機能です。主にAPIのリファレンス代わりとして使われており、詳細なドキュメントがなくとも呼び出し方や戻り値、引数等を把握することができます。

GraphQLの多くの実装では、デフォルトでIntrospectionが有効になっており、認証を必要とせずに本機能を利用できます。

## 影響

攻撃者にこの機能を悪用されると、対象のGraphQLに関する情報を取得され、他の脆弱性への攻撃につながってしまう可能性があります。

実際に報告された事例：

- [https://hackerone.com/reports/291531](https://hackerone.com/reports/291531)
- [https://hackerone.com/reports/969456](https://hackerone.com/reports/969456)
- [https://hackerone.com/reports/862835](https://hackerone.com/reports/862835)

## 検証方法

以下のようなクエリをエンドポイントに送信します。

```graphql
query IntrospectionQuery {
__schema {
    types {
      name
    }
  }
}
```

```graphql
{
  "data": {
    "__schema": {
      "types": [
        {
          "name": "Query"
        },
        {
          "name": "String"
        },
        {
          "name": "ID"
        },
    (...中略...)
        {
          "name": "__DirectiveLocation"
        }
      ]
    }
  }
}
```

上記クエリを送信し、正常にqueryTypeを含むレスポンスが返された場合、Introspection Queryが解釈された可能性が高いです。そして、上記レスポンスの通り、GraphQLシステムに存在する型の情報や、Query/Mutationの名前、それらを呼び出すのに必要な引数などの情報が取得できます。

なお、Introspection Queryで定義されているフィールドや、そのレスポンスについての詳細はGraphQL公式ドキュメント[1]を参照してください。

## 対策

Introspection Queryを無効化することを推奨します。
IntrospectionはAPI作成者がAPI情報を提供する目的で意図的に有効化している可能性もありますが、本番環境ではこの機能を有効とせずにドキュメントなど別の方法で情報を共有してください。

具体的な無効化の方法については各GraphQLライブラリによって異なるため、ドキュメントを参照してください。[2]

[1] GraphQL Introspection

[https://graphql.org/learn/introspection/](https://graphql.org/learn/introspection/)

[2] Introspectionを無効化する方法についてのドキュメント
[https://lab.wallarm.com/why-and-how-to-disable-introspection-query-for-graphql-apis/](https://lab.wallarm.com/why-and-how-to-disable-introspection-query-for-graphql-apis/)