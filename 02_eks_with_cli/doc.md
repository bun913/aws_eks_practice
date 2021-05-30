# EKS CLIを使用したセットアップ練習

本番環境では実際TerraFormやクラウドフォーメーションを利用するが、実際にCLIで動作させて、どのような動作になっているか感覚を掴む

## CLIインストール

```bash
# aws cli
brew install awscli
# aws configureでアクセスキーを設定しておくこと
# 確認 aws sts get-caller-identity

# aws-iam-authenticatorをインストール
brew install aws-iam-authenticator

# kubectl
brew install kubectl
# DockerDesktopをインストール済みであればそちらを使える

#eksctl
brew tap weaveworks/tap
brew install weaveworks/tap/eksctl
eksctl version
```

```bash
# eksワーカーノードのssh keyを作成しておく
ssh-keygen
# 今回は eks-demo.pemという名前で作成
```

## EKSクラスターを作成してみる

eksctlでコントロールプレーン・ワーカーノード・VPC・セキュリティグループなどを構築できる

```bash
eksctl create cluster \
    --name eks-from-eksctl \
    --version 1.16 \
    --region ap-northeast-1 \
    --nodegroup-name workers \
    --node-type t3.medium \
    --nodes 2 \
    --nodes-min 1 \
    --nodes-max 4 \
    --ssh-access \
    --ssh-public-key ~/.ssh/eks-demo.pem \
    --managed
# 15分くらいかかった
# 確認　
aws eks describe-cluster --name eks-from-eksctl --region ap-northeast-1
# コンソールでも確認したら、一旦削除
eksctl delete cluster --name eks-from-eksctl --region ap-northeast-1
```

マスターノードのEC2も立ち上がっているが、実際は表示されない。
マスターノードは各AZに分散されていて、AWS側で管理になる。

ワーカーノードをスケール設定していったり、カスタマイズしていく

