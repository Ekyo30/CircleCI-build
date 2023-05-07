# 第4回課題で行ったこと

### 新規VPC、ec2、RDSの作成

### cloud9からec2へ接続
~~~
$ ssh -i raisetech.pem ec2-user@ec2-43-206-158-226.
ap-northeast-1.compute.amazonaws.com
~~~

### Mysqlのセットアップ

### Ec2からRDSへ接続
~~~
$ mysql -u admin -p -h raisetech-db.cgnakqzvmqg2.
ap-northeast-1.rds.amazonaws.com
~~~
* パスワードを入力しても反応がなかったため、質問したところec2,RDSで使用しているVPCが違うことが判明。
同じもので再度作成したところ無事接続成功
