## 第11回課題

## ServerSpecを使って自動テストを実行する

* Gemfileに以下を追記

~~~
gem 'serverspec'
~~~

~~~
$ bundle
$ bundle install serverspec
$ mkdir serverspec-test
$ cd serverspec-test 
~~~
~~~
$ serverspec-init
Select OS type:

  1) UN*X
  2) Windows

Select number: 1

Select a backend type:

  1) SSH
  2) Exec (local)

Select number: 2
~~~

* 作成されたsample_spec.rbを編集

~~~
vi spec/localhost/sample_spec.rb
~~~

* 内容は下記

~~~
require 'spec_helper'

listen_port = 80
# nginxがインストールされているか
describe package('nginx') do
  it { should be_installed }
end
# 指定ポートがリッスン状態か
describe port(listen_port) do
  it { should be_listening }
end
# テスト接続して動作するか
describe command('curl http://127.0.0.1:#{listen_port}/_plugin/head/ -o /dev/null -w "%{http_code}\n" -s') do
  its(:stdout) { should match /^200$/ }
end
# nginxが自動起動設定されていて起動しているか
describe service('nginx') do
  it { should be_enabled }
  it { should be_running }
end
# unicornが自動起動設定されていて起動しているか
describe service('unicorn') do
  it { should be_enabled }
  it { should be_running }
end
~~~

 * 自動起動設定をしていなかったので、この機会に行おうと思ったため上記内容にした

~~~
# nginxの自動起動設定
$ sudo systemctl enable nginx
$ sudo systemctl nginx
~~~
~~~
# unicornの自動起動設定
$ sudo vi /etc/systemd/system/unicorn.service
~~~
~~~
[Unit]
Description=The unicorn process
Before=nginx.service
After=network.target remote-fs.target nss-lookup.target

[Service]
User=ec2-user
WorkingDirectory=/home/ec2-user/sub/raisetech-live8-sample-app
StandardError=/home/ec2-user/sub/raisetech-live8-sample-app/log/unicorn.log 
SyslogIdentifier=unicorn
PIDFile=/home/ec2-user/sub/raisetech-live8-sample-app/unicorn.pid
Type=simple
Restart=on-failure

Environment=UNICORN_CONF=/home/ec2-user/sub/raisetech-live8-sample-app/config/unicorn.rb
Environment=BUNDLE_GEMFILE=/home/ec2-user/sub/raisetech-live8-sample-app/Gemfile
ExecStart=/bin/bash -l -c 'bundle exec unicorn_rails -c ${UNICORN_CONF} -D'
ExecStop=/bin/kill -QUIT $MAINPID
# The unicorn process is gracefully restarted with `kill -USR2 <PID>` along with the setting
# in the before_fork hook in config/unicorn.rb.
# ref: https://tachesimazzoca.github.io/wiki/rails3/unicorn.html
ExecReload=/usr/bin/kill -USR2 $MAINPID

[Install]
WantedBy=multi-user.target
~~~

* はまってしまったのでこれは別で進めることにする

~~~
$ sudo systemctl enable unicorn.service
$ reboot
~~~

* 再度EC2へ接続

~~~
$ sudo systemctl status unicorn
● unicorn.service - The unicorn process
   Loaded: loaded (/etc/systemd/system/unicorn.service; enabled; vendor preset: disabled)
   Active: failed (Result: start-limit) since 月 2023-03-27 10:59:49 UTC; 1min 18s ago
  Process: 6454 ExecStop=/bin/kill -QUIT $MAINPID (code=exited, status=1/FAILURE)
  Process: 6193 ExecStart=/bin/bash -l -c bundle exec unicorn_rails -c ${UNICORN_CONF} -D (code=exited, status=0/SUCCESS)
 Main PID: 6193 (code=exited, status=0/SUCCESS)
~~~
~~~
# serverspec-initにてrakefileが格納してあるところで
$ rake
~~~
結果は下記画像
<img width="1680" alt="第11回課題" src="https://user-images.githubusercontent.com/111736198/227927441-2b3477ee-3736-4c53-9840-9e8c26c1ef24.png">

## 感想
* serverspecの設定はそこまで複雑ではなかったがserverspec-initのbackend typeのところで最初はssh接続をしているためsshを選択していて、うまく動作しなかった。実行するのはこのEC2上であるからlocalを選択すると調べてからわかった。ここは気をつけたい。
* unicornの自動化ではまってしまったが、今回の課題ではないので課題外で取り組んでいこうと思う。

