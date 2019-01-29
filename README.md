Docker for Mac で Ruby on Rails アプリの開発環境

## Railsアプリ用のディレクトリを作成
workディレクトリ(なんでも構いません)を作成して
```
mkdir work
cd work
```
githubからクローンしてくる
```
git clone https://github.com/nuovotaka/docker-rails4.git
cd docker-rails4
```

## コンテナにRailsアプリを作成

「docker-compose run」コマンドで「web」コンテナに「rails new」を行う。
```
docker-compose run --rm web rails new . --force --database=postgresql --skip-bundle
```
実行後、ディレクトリの中にRailsのファイル群が作成されている

## Gemfileの変更

バージョン情報を削除する
```
gem 'pg'
```

以下の行のコメントを外す
```
gem 'mini_racer', platforms: :ruby
```
Gemfileの変更を適用するためには以下のコマンドを実行
```
docker-compose build
```

## config/database.ymlの設定

config/database.yml内の記述を下記のように変更する
```
default: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS")  { 5 } %>

  host: <%= ENV.fetch('DATABASE_HOST') { 'localhost' } %>
  port: <%= ENV.fetch('DATABASE_PORT') { 5432 } %>
  username: <%= ENV.fetch('DATABASE_USER') { 'root' } %>
  password: <%= ENV.fetch('DATABASE_PASSWORD') { 'password' } %>

```

## コンテナの実行＆ブラウザの確認

最初はデータベースを作成する（次回からは必要ない）
```
docker-compose run --rm web rake db:create
```

### コンテナの実行

```
docker-compose up
```

### コンテナの確認

```
docker-compose ps
```

localhost:3000 でRailsのウェルカム画面が表示されるのがわかると思う。

## Rails アプリを作成する方法

例　メモアプリの作成
別タブで以下のコマンドを実行する
```
docker-compose run web rails g scaffold Memo title:string body:text
```

基本的にRailsコマンドの最初に「docker-compose run web」をつけてやればコンテナ上で実行され、Mac側にも反映されます。

### アプリの確認

localhost:3000/memos でアクセスするとアプリが作成されているのがわかる。

## 注意点

Gemfileを変更したら、buildでイメージ作成してから、コンテナ実行すること
```
$ docker-compose build
$ docker-compose up
```

## コンテナの停止と削除
Dockerに付随してくるkitematicで停止と削除を行う

## コンテナのイメージの削除
Dockerに付随してくるkitematicでMy Imagesからイメージの削除を行う

## 再起動
古いプロセスの削除
```
rm -rf tmp/pids/server.pid
```
そしてupする
```
docker-compose up
```

## mysqlへの接続
[TablePlus](https://tableplus.io/)を使ってデータベースを確認(Mac)
