# RaiseTech-Task

## 概要

- RaiseTechAWSフルコースでの作成物をまとめたリポジトリである。

- CircleCIで以下の動作を自動処理するようにしている。（EIPを使用、動作させるには割り当てが必要）
　
  - CloudFormationでAWSリソースを作成
　　
  - Ansibleを使用し、作成したEC2へGitのインストール
  
  - ServerspecでGitがインストールされているかを確認

- [task_report](https://github.com/Ekyo30/RaiseTech-Task/tree/main/task_report)には課題報告用に作成したマークダウンをまとめている。

## AWS構成図
![最終AWS構成図](https://user-images.githubusercontent.com/111736198/235822953-8101f1da-4799-4602-aebc-b7ef82ca7664.png)

## フロー図
![フロー図改訂版](https://user-images.githubusercontent.com/111736198/235887574-7570f76b-91f7-4453-b7a9-ac4ea9369116.png)



