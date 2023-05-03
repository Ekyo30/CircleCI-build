## 第13回課題
## circleciのconfigにansible、serverspecの処理を追加

* 今回のやりたいことは以下の２つ。

* ansibleでssh接続したec2へgitのインストール。

* serverspecでインストールされているかのテスト。

* コントロール用ターゲット用に２つのec2を作成。

* コントロールに接続して、ansibleをインストール。

~~~
$ sudo amazon-linux-extras install ansible2
~~~

* playbookとinventoryを作成。

~~~
$ mkdir ansible 
$ cd ansible
$ touch playbook.yml inventory
~~~

* 作成したplaybookをドライラン

~~~
$ ansible-playbook playbook.yml -i inventory --check
~~~

* syntax-checkは行わなくてもいいかと思ったが、ドライランだとスキップされてしまう箇所があるためその箇所の確認には必要だと感じた。

* inventoryの指定の方法がわからず何度か躓いたが、-i で指定できると学んだ。

* ドライランが成功したので、circleciのconfigに処理を追加。

* 用意してあるサンプルと書き方を揃えるためにorbsを使用した。

* 実行に成功したので、serverspecの追加を行う

* コントロールにrubyを入れていなかったのでインストール

* 作業ディレクトリ、Gemfileを用意して、初期設定

~~~
$ mkdir serverspec
$ cd serverspec
~~~

~~~
source 'https://rubygems.org'

gem 'serverspec'
gem 'rake'
~~~

~~~
$ bundle install 
~~~

~~~
$ serverspec-init
~~~

* sample_spec.rbを編集して、gitがインストールされているかのテストを記載

* bundle exec rake specを実行してもSSH接続が失敗していたので、configに秘密キーを記載

* 成功が確認できたのでcircleciに処理を追加

* circleciのconfigを書き直してもうまく動作しなかった。

* 原因はcircleciからうまくターゲットに接続できていなかったためだと思われる。

* 接続に必要な情報を改めて見直し、circleciに登録しているsshkeyとserverspec-init実行時のtargethostnameを任意の名前にしていたためターゲットのIPに設定しなおした。

* ansibleが正常に動いたため問題ないと思っていた箇所だったが、ansibleに関してはinventoryでhostの情報を記載していたため動いていたのだと思われる。

* 変更したところエラー内容が変わり、ed25519、bcrypt_pbkdfのgemが必要とのことなので追加。

* それでも失敗していたので、エラー内容をみたところuserを指定していないのが原因のようだったので、spec_helper.rbでec2-userを指定したところ無事成功した。

<img width="1420" alt="task13成功" src="https://user-images.githubusercontent.com/111736198/233903282-083045fd-feda-443d-8932-461d25496923.png">

## 感想
* 最初はデプロイまでやろうと思っていたが、rubyのインストールをansibleで行おうとした際に何度もはまってしまいうまくできなかったのでとりあえず確実に成功するものに変更した。やっていくうちにrubyのインストールはcircleci上でできるからこれを使えばデプロイまでできたんじゃないかと思ったため今後の課題としたい。

* あとはhostnameはIPにしておいた方が無難だと感じた。
