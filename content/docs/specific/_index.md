---
title: "GraphQLで起こりやすい問題"
description: "GraphQL診断ガイドライン"
weight: 3
type: docs
bookToc: false
---

# GraphQLで起こりやすい問題
<!-- textlint-disable prh -->
前述のような特徴を持ったGraphQLですが、その特徴ゆえに以下のような問題が起こりやすくなくっています。
<!-- textlint-enable prh -->
- [Introspection Query](introspection_query)
- [GraphQLによるDoS攻撃](dos)
- [レースコンディション](race_condition)

また、Webアプリケーションに共通する脆弱性についても影響を受ける恐れがあります。このうち特に重要なものについては次章で説明します。
