## 第7回課題

## 現在作成している環境の脆弱性
* 第5回課題で作成したアプリデプロイ環境の脆弱性を検討する
* 構成図は以下のようになっている
![第5回課題-ページ2](https://user-images.githubusercontent.com/111736198/214258520-6f341e79-6173-4f6c-a6d4-8ad7f98415e0.jpg)

##webアプリケーションの脆弱性と対策
* デプロイする際にHTTPアクセスを使用していることが挙げられる。こちらに関してはACMなどを使い、SSL証明書を発行し、HTTPS限定にすることで対策できる。また、WAFを使い、ALBにルールを設置することででよりセキュリティを強めることもできる。  

## OS、ミドルウェアの脆弱性と対策
* まず脆弱性を把握する必要があるため日本のCVSSデータベース[JVN](https://jvndb.jvn.jp/)を確認したりAmazon Inspectorの導入をする。具体的な対策はそれぞれに合わせて行う必要がある。実際の画面に関しては[公式サイト](https://docs.aws.amazon.com/ja_jp/inspector/latest/user/getting_started_tutorial.html)や[こちら](https://qiita.com/mksamba/items/f94be02097d461d98388)の実際に使用している方のサイトで確認をした。

## ネットワーク、その他の脆弱性と対策
* こちらに関しても、設定時に気を配っていないため脆弱性の把握から行う必要がある。その際に使えるのがSecrity Hubである。コストの関係上、実際に操作はしていないが、[公式サイト](https://docs.aws.amazon.com/ja_jp/securityhub/latest/userguide/what-is-securityhub.html)や[こちら](https://zenn.dev/prayd/articles/5435a9798d3701)の実際に運用している方のサイトを確認した。
* 認証情報に関しては、IAMはMFAでログイン制限をかけている。
* AWSのサービスを使う上でIAMの情報が漏れたら元も子もないからここには特に気をつける必要がある。

## 感想
* 現段階ではそこまで使う機会はないがしっかりと活用できるように内容の理解は進めていきたい。現段階だと一人の問題で済む範囲ではあるが、現場に出るとなるとそうもいかないので気をつけていきたい。
