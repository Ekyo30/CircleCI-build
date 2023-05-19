# CircleCIを用いた自動処理の作成

## 概要

- CircleCIで以下の動作を自動処理するようにしている。（EIPを使用、動作させるには割り当てが必要）
　
  - CloudFormationでAWSリソースを作成
　　
  - Ansibleを使用し、作成したEC2へGitのインストール
  
  - ServerspecでGitがインストールされているかを確認

- [task_report](https://github.com/Ekyo30/RaiseTech-Task/tree/main/task_report)にはオンラインスクールでのアウトプットをまとめている。

## AWS構成図
![最終AWS構成図](https://user-images.githubusercontent.com/111736198/235822953-8101f1da-4799-4602-aebc-b7ef82ca7664.png)

## フロー図
![フロー図改訂版](https://user-images.githubusercontent.com/111736198/235887574-7570f76b-91f7-4453-b7a9-ac4ea9369116.png)

## CircleCI動作確認
<img width="1676" alt="構築成功" src="https://github.com/Ekyo30/CircleCI-build/assets/111736198/bb20b622-2bf8-4556-af9f-c50641094156">

## CFn作成リソース一覧
<img width="1350" alt="作成リソース一覧" src="https://github.com/Ekyo30/CircleCI-build/assets/111736198/a0ffe171-81e8-4f05-a83b-54bb99c79faa">

