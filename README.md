# base-docekr
 
# サイト環境系  
## 言語   
PHP7系  7.4	   

## フレームワーク  
Laravel Framework 6.12.0 
https://readouble.com/laravel/6.x/ja/installation.html  

## 画面側  
Vue.js  

## CI  
CircleCI    
https://app.circleci.com/

## Git  
https://github.com/ysi-m-yobit 

## IDE
Visual Studio Code

# ローカル環境の構築の仕方
## 1.dockerを入れる
docker.ioにアクセスし、アカウントを登録後。  

MAC : docker for macをインストール  
Windows : docker for Windowsをインストール  

## 2.dockerのアクセストークンの設定
docker.ioのアカウントの設定からSecurityを選択。  
アクセストークンを作成する。  

作成後、ターミナル or コマンドプロンプトで下記のコマンドを実行。

```bash
docker login --username  {ユーザー名}
Password: {アクセストークン}
```

## 3.gitリポジトリのclone
base-dockerをcloneする。  
サブモジュールの初回clone、および最新のブランチに合わせるため、   
base-dockerのrootディレクトリで以下のコマンドを実行。  
```bash
# gitから落としてきたソースの場所に移動
cd base-docker
# submoduleの初回ダウンロード
git submodule update --init --recursive
# submodule内を指定のリモートブランチに更新
git submodule foreach git pull origin master
```

## 4.docker実行
```bash
# gitから落としてきたソースの場所に移動
cd base-docker
# dockerイメージをローカルに持ってきたりphp設定したり。構築する
# 〇〇 is already loaded! とか言われるけど多分大丈夫。  
docker-compose build

# dockerコンテナをバックグラウンドで実行
docker-compose up -d
```

[コマンド参考サイト](https://qiita.com/tomo62/items/d966908cbc79a4c52dc7)


## 5.Laravelの設定
`base-docker/www/html/base-laravel/.env.example` ファイルをコピーして貼り付け、`.env` に名前を変更しておく。  
dockerコンテナにログインしてcomposerを元にPHPのライブラリ達をダウンロード

```bash
# php-fpmのdockerコンテナにログイン
docker-compose exec php-fpm sh
# (コンテナ内)ソースがある場所に移動
cd base-larave
# composerで管理されているライブラリをインストール
composer install
# Laravelのエンクリプタを使用する準備
php artisan key:generate

```

## 6.確認
ローカルPCから `http://localhost/` にアクセスして動作確認。画面が表示されたらOK!


# VSCodeの設定

## 拡張機能
 - PHP IntelliSense
 - PHP Debug

## XDebugの接続設定
デバッグのタブから、構成をPHPで追加。  
launch.jsonで以下のようにpathMappingsを追加。  
(WorkSpaceはbase-dockerのリポジトリフォルダを読み込んでいることが前提)
```
"configurations": [
    {
        "name": "Listen for XDebug",
        "type": "php",
        "request": "launch",
        "port": 9001,
        "pathMappings": {
            "/var/www": "${workspaceRoot}/www"
        }
    },
```
