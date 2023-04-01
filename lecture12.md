## 第12回課題
## サンプルのcircleciを動作するように組み込む
* まずは[公式ドキュメント](https://circleci.com/docs/ja/2.0/getting-started/)に従ってサンプルを作り動作を確認。
* 次に課題提出用のリポジトリにて設定を行い、config.ymlの中身をサンプルに置き換えてからcommitからのmerge
* 現状テストはfailedになっている。
<img width="1421" alt="第12回課題エラー1回目" src="https://user-images.githubusercontent.com/111736198/229302768-ddbb2878-9392-4946-a8b7-b5128dd7a9ea.png">
* 原因は、cloudformation/*.ymlが見つからないということなので第10回の課題で作成した.ymlの格納場所をcloudfomationディレクトリに変更。また、拡張子を.yamlにしていたのでこちらもそれぞれ.ymlに変更した。
* 変更を加えた上で結果を確認したが再びfailedになった。
<img width="1419" alt="第12回課題エラー2回目" src="https://user-images.githubusercontent.com/111736198/229302816-c9cce673-bceb-45eb-a495-ff2d96443b50.png">
* 原因はRDSの.ymlにマスターパスワードをハードコーディングしていることだった。この機会にsecretmanagerでマスターユーザー、パスワードを管理することにする。
* 以下をRDS.ymlに追記

~~~
    RDSSecret:
        Type: "AWS::SecretsManager::Secret"
        Properties:
          GenerateSecretString:
            SecretStringTemplate: '{"username": "admin"}'
            GenerateStringKey: "password"
            PasswordLength: 16
            ExcludeCharacters: '"@/\'
            
    Instance:
        Type: "AWS::RDS::DBInstance"
        Properties:
            MasterUsername: !Join ['', ['{{resolve:secretsmanager:', !Ref RDSSecret, ':SecretString:username}}' ]]
            MasterUserPassword: !Join ['', ['{{resolve:secretsmanager:', !Ref RDSSecret, ':SecretString:password}}' ]]

    SecretRDSInstanceAttachment:
       Type: "AWS::SecretsManager::SecretTargetAttachment"
       Properties:
         SecretId: !Ref RDSSecret
         TargetId: !Ref Instance
         TargetType: AWS::RDS::DBInstance
~~~

* 追記した上でテストをしたがfailedになってしまった。
<img width="1419" alt="第12回課題エラー3回目" src="https://user-images.githubusercontent.com/111736198/229302884-9e1b8054-9786-46df-b2a1-8d2091993e3d.png"> 
* 記載した内容自体は間違っていないと思うのでスペースが正しいかどうかを見直したところ、何箇所か間違っていたのでそこを修正。
* その後テストを行ったところ無事成功した。
<img width="1421" alt="第12回課題成功" src="https://user-images.githubusercontent.com/111736198/229303270-6635e628-d769-4872-a8fe-1bcd9cd1465d.png">

## 感想
* 導入の仕方や操作もそこまで複雑ではなかった。UIが見やすく原因特定もしてくれるのでserverspecよりも使いやすい印象を受けた。
