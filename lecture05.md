# 第5回課題
 
## 組み込みサーバーでの起動

1. 環境構築（始めに取り組んだ時から時間を置いてしまったため曖昧なところがあるため省略）
　

2. サンプルアプリケーションのインストール
3. gemのインストール（sassc 2.4.0 は個別にインストール）
4. config/database.ymlの編集

~~~
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: RDSのユーザー名
  password: RDSのパスワード
  host: RDSのエンドポイント
development:
  <<: *default
  database: raisetech_live8_sample_app_development
  socket: /var/lib/mysql/mysql.sock
~~~

5. プリコンパイルの実施（この際production環境を指定していたことで何度もエラーが発生していた。指定する環境には要注意）

~~~
bundle exec rails assets:precompile RAILS_ENV=development
~~~

6. サーバーの起動

~~~
rails s -p 3000 -b 0.0.0.0
~~~

<img width="1680" alt="組み込み起動" src="https://user-images.githubusercontent.com/111736198/214257454-6f8f3b4d-4047-4287-a1bd-c013bb170601.png">

## サーバーアプリを分けての起動

1. Nginxのインストール、起動

~~~
sudo amazon-linux-extras install nginx1
sudo systemctl start nginx
~~~

2. Nginxの設定ファイルの作成

~~~
sudo vi /etc/nginx/conf.d/rails.conf
~~~

以下設定内容

~~~
upstream unicorn {
    server  unix:/home/ec2-user/raisetech-live8-sample-app/unicorn.sock;
}

server {
    listen       80;
    server_name  Elastic IP;

    access_log  /var/log/nginx/access.log;
    error_log   /var/log/nginx/error.log;

    root /home/ec2-user/raisetech-live8-sample-app/public;

    client_max_body_size 100m;
    error_page  404              /404.html;
    error_page  500 502 503 504  /500.html;
    try_files   $uri/index.html $uri @unicorn;

    location @unicorn {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_pass http://unicorn;
    }
~~~

3. unicornはインストール済みのためそのまま起動

~~~
bundle exec unicorn_rails -c config/unicorn.rb -D
~~~

<img width="1680" alt="unicorn,nginxでの起動" src="https://user-images.githubusercontent.com/111736198/214257826-c4322335-469a-4ca1-94dc-ae9b58108d0a.png">

## ELBの追加
1. ターゲットグループとALBの作成
公式サイトを参考（https://docs.aws.amazon.com/ja_jp/elasticloadbalancing/latest/application/create-application-load-balancer.html）

2. development.rbへホストの接続許可を記載

~~~
Rails.application.configure do

  config.hosts << "<許可したいホスト名>"

end
~~~

3. DNSへ接続

<img width="1680" alt="ELB追加接続" src="https://user-images.githubusercontent.com/111736198/214258014-76417cc1-a5ef-4af9-9d73-96036f06c7ea.png">

##S3の追加（画像の保存先をS3に変更する）
1. S3バケットの作成（参考サイト　https://qiita.com/ysda/items/49fa6e8318c874a57b9e）

2. config/environments/development.rbの内容変更

~~~
config.active_storage.service = :amazon
~~~

3. config/storage.ymlの内容変更

~~~
amazon:
 service: S3
 access_key_id: <%= Rails.application.credentials.dig(:aws, :access_key_id) %>
 secret_access_key: <%= Rails.application.credentials.dig(:aws, :secret_access_key) %>
 region: バケットのリージョン
 bucket: 作成したバケット名
~~~

4. credentialsの設定
（何度かここでエラーが発生したがバケットポリシーの設定とcredentialsの書き方が原因だと思われる。）

~~~
EDITOR=vi rails credentials:edit
~~~

<img width="1680" alt="S3保存確認" src="https://user-images.githubusercontent.com/111736198/214258210-c541de52-6248-44db-a437-f53da67268b4.png">
<img width="1680" alt="エラー解消起動成功" src="https://user-images.githubusercontent.com/111736198/214258288-db5d17dd-8140-40e1-9beb-0453213d7776.png">

## AWS構成図

![第5回課題-ページ2](https://user-images.githubusercontent.com/111736198/214258520-6f341e79-6173-4f6c-a6d4-8ad7f98415e0.jpg)

## 課題の感想
* 今までの比ではないぐらいさまざまなエラーに遭遇した。とにかく各種エラーログがどこにあるのかを見るのが大切だと感じた。

