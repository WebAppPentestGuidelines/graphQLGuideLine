# 診断に役立つツール

## GraphQL クライアント

GraphQLに対応したクライアントを利用することで、対象のGraphQL構成の把握や調査に役立ちます。プロキシツールと併用することで手動での検査をする際にも有用です。

- Altair
    - [https://altair.sirmuel.design/](https://altair.sirmuel.design/)
- Insomnia
    - [https://insomnia.rest/products/insomnia](https://insomnia.rest/products/insomnia)
- GraphiQL
    - [https://github.com/graphql/graphiql](https://github.com/graphql/graphiql)
- graphql-voyager
    - [https://github.com/APIs-guru/graphql-voyager](https://github.com/APIs-guru/graphql-voyager)

## プロキシツール

Webアプリケーション一般のデバッグや脆弱性診断によく用いられる、HTTPレベルでの操作をしやすくするためのツールです。

- Burp Suite
    - [https://portswigger.net/burp](https://portswigger.net/burp)
- Fiddler
    - [https://www.telerik.com/fiddler](https://www.telerik.com/fiddler)
- OWASP ZAP
    - [https://www.zaproxy.org/](https://www.zaproxy.org/)
- Packet Proxy
    - [https://github.com/DeNA/PacketProxy](https://github.com/DeNA/PacketProxy)

## 学習サイト/環境

GraphQLに関係した脆弱性について学べるサイトやツールは、攻撃者視点でWebアプリケーションを見る上で有用です。

- Damn-Vulnerable-GraphQL-Application(DVGA)
    - [https://github.com/dolevf/Damn-Vulnerable-GraphQL-Application](https://github.com/dolevf/Damn-Vulnerable-GraphQL-Application)
- hacker101
    - [https://www.hackerone.com/hackers/hacker101](https://www.hackerone.com/hackers/hacker101)

## 自動スキャンツール

手動で見つけられない脆弱性を機械的に探す場合や、網羅的に探す場合に有用です。

- inQL (Burp Suite Extender)

Webアプリケーションの診断でよく用いられるBurpにおいて、GraphQLの診断をサポートしてくれるExtensionです。

[https://github.com/doyensec/inql](https://github.com/doyensec/inql)

- OWASP ZAP Addon

オープンソースで提供されているWebアプリケーション診断ツールであるOWASP ZAPにおいて、GraphQLの診断をサポートしてくれるAddonです。

[https://github.com/zaproxy/zap-extensions/tree/main/addOns/graphql](https://github.com/zaproxy/zap-extensions/tree/main/addOns/graphql)

- GraphQLmap

スキーマのダンプやファジングが可能な、侵入テストの目的でgraphqlエンドポイントと対話するためのスクリプトエンジンです。

[https://github.com/swisskyrepo/GraphQLmap](https://github.com/swisskyrepo/GraphQLmap)

- graphql-path-enum

スキーマやタイプを特定するためのさまざまな方法をリストするツールです。

[https://gitlab.com/dee-see/graphql-path-enum](https://gitlab.com/dee-see/graphql-path-enum)