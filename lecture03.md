# 第3回課題報告
## 行ったこと
* サンプルアプリを git clone

* Mysqlのセットアップ

~~~
https://onedrive.live.com/?authkey=%21AJUub1X2ubpj9Q
c&cid=D2DFE9880240895A&id=D2DFE9880240895A%2126879&pa
rId=D2DFE9880240895A%211376&o=OneUp
~~~

* Bundleのinstall

* Development.rbにてホストの接続を許可する

~~~
Rails.application.configure do
config.hosts << "<許可したいホスト名>"
end
~~~
* socketの正しいパスを指定(下記コードでパスを探す)

~~~
$ mysql_config --socket
~~~

* rootのpasswordを更新、datebase.ymlへ書き込む

~~~
$ sudo mysql_secure_installation
~~~


* dbのcreate、migrate

~~~
$ rails db:create
$ rails db:migrate
~~~
* yarnのインストール

~~~
npm install -g yarn
~~~
* webpackerのinstall,compile

~~~
rails webpacker:install
rails webpacker:compile
~~~

## 調べたこと
####APサーバー　
* Puma 5.6.4
* 終了後はアクセス不可

####DBサーバー
* MySQL 8.0.30
* 終了後はアクセス不可

#### Railsの構成管理ツール
* Bundler 2.3.14

## 学んだこと
* エラーメッセージは大事、内容を見てある程度理解してから検索すると必要な答えに近づける