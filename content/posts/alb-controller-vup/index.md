---
title: "AWS Load Balancer ControllerのVUP"
date: 2022-03-10T08:22:40+09:00
draft: false
description: VUPの手順です
tags: [aws, eks, kubernetes, infra]
---

## できたこと
以前実施した[AWS Load Balancer Controllerの構築](../kube-with-aws)後にVUPがあったので雑に実施します。
雑にとあるのは、わざわざダウンタイムを考えずに、手順の差分でアップデートできるかを見ると言うことです。

## 比較対象
- [2.2](https://kubernetes-sigs.github.io/aws-load-balancer-controller/v2.2/deploy/installation/)
- [2.4](https://kubernetes-sigs.github.io/aws-load-balancer-controller/v2.4/deploy/installation/)

[tags](https://github.com/kubernetes-sigs/aws-load-balancer-controller/tags)からそれまでの差分はわかります。

## 環境
EKS 1.20

## 作業ログ
1つ1つ挙げていきます

### Using metadata server version 2 (IMDSv2)
IMDSv2がいきなりわからないので[クラメソ](https://dev.classmethod.jp/articles/ec2-imdsv2-release/)の記事で勉強しました

適当にクラスタ内のEC2で設定を確認
```bash
$ aws ec2 describe-instances --instance-id i-9999999999999999 --query "Reservations[*].Instances[*].MetadataOptions"
[
    [
        {
            "State": "applied",
            "HttpTokens": "optional",
            "HttpPutResponseHopLimit": 2,
            "HttpEndpoint": "enabled"
        }
    ]
]
```

インフラ設定で2にしてたのかわからないですが、何もしなくてよかったです。
[元のIssue](https://github.com/kubernetes-sigs/aws-load-balancer-controller/issues/2231)のように、1にして（おり、かつIMDSv2になって？）いる場合は注意した方がいいです。

### IAM Permissions
curlしたものの差分を見ると、これだけでした。ちょっとだけ条件がついたくらい。
[元のIssue](https://github.com/kubernetes-sigs/aws-load-balancer-controller/issues/2276)
```diff
-                "iam:CreateServiceLinkedRole",
+                "iam:CreateServiceLinkedRole"
+            ],
+            "Resource": "*",
+            "Condition": {
+                "StringEquals": {
+                    "iam:AWSServiceName": "elasticloadbalancing.amazonaws.com"
+                }
+            }
+        },
+        {
+            "Effect": "Allow",
+            "Action": [
```

何もしなくてもいいとは思いますが、一応ロールを更新(労力変わらないのでマネコンで)

あと、eksctlにリージョンのオプションが追加されてますが、まあ大した変更ではないのでスルー

### Add Controller to Cluster
cert-manegerのインストールは、eksのサポート範囲変更により簡略化

最後にapplyするyamlの差分を見たところ、細かな変化はあるもののここが１番の変化かなと。
https://kubernetes-sigs.github.io/aws-load-balancer-controller/v2.4/guide/ingress/ingress_class/#deprecated-kubernetesioingressclass-annotation

本格的にdeprecatedになるのはメジャーアップくらいの段階だとは思いますが、exampleの変更のように、Ingressを変えておいてもいいと思います。

- [echo server v2.2](https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.2.0/docs/examples/echoservice/echoserver-ingress.yaml)
- [echo server v2.4](https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.4.0/docs/examples/echoservice/echoserver-ingress.yaml)

before
```yaml
metadata:
  name: echoserver
  namespace: echoserver
  annotations:
    kubernetes.io/ingress.class: alb
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/tags: Environment=dev,Team=test
spec:
  rules:
  ...
```

after
```yaml
metadata:
  name: echoserver
  namespace: echoserver
  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/tags: Environment=dev,Team=test
spec:
  ingressClassName: alb
  rules:
  ...
```
