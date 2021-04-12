---
title: "RDSに関する勉強"
date: 2021-04-12T14:50:16+09:00
draft: false
description: AWS、RDSに関する端っこに書いてあることまとめ
tags: [aws, rds, serverless]
---

### Aurora Serverless

参考記事

- https://service.plan-b.co.jp/blog/tech/28232/

### サブネットグループ

すごい一般的な名前に見えるけど、それぞれのサービスで定義されてるようだ

- [RDS](https://docs.aws.amazon.com/ja_jp/AmazonRDS/latest/UserGuide/CHAP_Tutorials.WebServerDB.CreateVPC.html#CHAP_Tutorials.WebServerDB.CreateVPC.DBSubnetGroup)
- [ElastiCache](https://docs.aws.amazon.com/ja_jp/AmazonElastiCache/latest/mem-ug/SubnetGroups.Creating.html)
- [Dynamo DB](https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/DAX.create-cluster.console.create-subnet-group.html)
