# Ruby (On Rails)
## RubyとRailsって何
- 日本人が開発したプログラミング言語
- さまざまな用途があるが、特に多いのはWebアプリケーションのサーバーサイドプログラム
- RubyといえばRailsというぐらいには、Ruby On Railsが強力なWebアプリケーションフレームワークだったりする
  - GithubはRailsでつくられてる
  - MVC関係が標準で用意されている
  - 「ユーザー認証」「セキュリティ対策」みたいな機能も簡単に追加できるらしい
- そう言われるとそんな機能どうやって気軽に追加するのかが気になってきた


## Railsをインストールする
- Linuxカーネル上にRuby2.5.3を入れてある前提。まあDockerが使える我々なら下準備には困らない故。
- `gem install rails`でRails本体をインストール
- 以上。


## Webアプリを新規作成する
- `rails new testapp`を実行。途中で端末を実行しているユーザーのパスワードを要求されるらしい。
  - これにより端末で現在開いているフォルダの直下に'testapp'が作成されるらしい。
- 以上。


## Webアプリを実行する
- `rails s`で実行。
- 以上。


## アプリの中身
- testapp
  - app
    - controllers コントローラーが入ったフォルダ
      - application_controller.rb
    - models モデルが入ったフォルダ
      - application_record.rb
    - views ビューが入ったフォルダ
      - application_html.erb
  - config
    - database.yml DB接続設定ファイル
    - routes.rb ルーティングを行うファイル
  - db
    - migrate データベースのテーブルを生成するファイルが入ったフォルダ
- 要するにRailsも例に漏れずお約束のMVCなわけですね。


## scaffoldでWebアプリを一気につくる
- なんか土台部分だけだったら超お手軽に作れるらしいですよ。
- `rails g scaffold greeting(モデル名) name(カラム名):string(カラムの型) message(カラム名):text(カラムの型)`
- このコマンドだけでモデルやコントローラーなどの必要なものを一式そろえてくれる。こわい。
- その後 `rails db:migrate` を実行することで、Railsに内蔵されているSQLite3に勝手に必要なテーブルが作成される。こわい。
  - ↓みたいな内容のgreeting(モデル名)テーブルがあっという間に生成されてしまう。


|id|name|message|created_at|updated_at|
|--|----|-------|----------|----------|
| 1|"me"|"Hello"|2019-01-03 05:20:38|2019-01-03 05:20:38|

- 'rails s'でつくったアプリを実行できる
- たったこれだけで「新規投稿」「編集」「削除」「1件表示」「一覧表示」ができるWebアプリケーションのできあがり。なにそれ怖い。


## ユーザー認証の実装
- なんかこっちもお手軽らしいよ。嘘だろおい。
- gemfile(ライブラリをまとめて導入するための指示書みたいなファイル)に `gem 'devise'`を記述する。
- `bundle exec rails g devise:install` でdeviseの設定ファイルを生成する。
- config/environments/development.rb に以下を追記
  - `config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }`



## 出典
- "偶然"手元にあった書籍『スラスラ読める Ruby ふりがなプログラミング』（2019年 リブロワークス 著）
- [Rails deviseによるユーザー認証 メールによる認証、emailとusernameのどちらでもログイン可能にするまで - Qiita](https://qiita.com/shizuma/items/c8c2e71af8c1dcf3d1c2)