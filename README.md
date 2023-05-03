# RaiseTech-Task

## 概要

- RaiseTechAWSフルコースでの作成物をまとめたリポジトリである。

- CircleCIで以下の動作を自動処理するようにしている。
 - CloudFormationでAWSリソースを作成
 - Ansibleを使用し、作成したEC2へGitのインストール
 - ServerspecでGitがインストールされているかを確認

- [task_report](https://github.com/Ekyo30/RaiseTech-Task/tree/main/task_report)には課題報告用に作成したマークダウンをまとめている。

## AWS構成図
![最終AWS構成図](https://user-images.githubusercontent.com/111736198/235822953-8101f1da-4799-4602-aebc-b7ef82ca7664.png)

## フロー図

![フロー図](https://user-images.githubusercontent.com/111736198/235823024-921a2d32-69ed-4b02-a518-2c197f82bf44.png)