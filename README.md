# Ruby練習用の開発環境構築

## 環境構成
Windows上にWSL2 (Ubuntu) を有効化し、そこに Ruby を構築。
さらに Docker Desktop を導入し、WSL2 上でコンテナ環境も利用可能にする。

### 1. 手順

### Step 1.  WSL2 を有効化
Windows PowerShell（管理者）で実行

確認：` wsl --install -d Ubuntu-22.04 `

再起動 → Ubuntu ユーザー作成

確認：` wsl -l -v   # Ubuntu-22.04 が Version 2 であること `

### Step 2. Docker Desktop をインストール
Windowsに Docker Desktop を導入

設定 → Use the WSL 2 based engine をON

設定 → Resources → WSL Integration で Ubuntu-22.04 を有効化

確認：` docker run hello-world `

### Step 3. Ubuntu(WSL2) 側で開発ツール導入

```
sudo apt update
sudo apt install -y build-essential git curl \
  libssl-dev libreadline-dev zlib1g-dev pkg-config \
  libffi-dev libyaml-dev libgdbm-dev libdb-dev uuid-dev \
  libsqlite3-dev
```

### Step 4. rbenv で Ruby をインストール
```
# rbenv 本体
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init - bash)"' >> ~/.bashrc
exec $SHELL

# ruby-build プラグイン
git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build

# Ruby インストール
rbenv install 3.3.0
rbenv global 3.3.0
ruby -v   # => ruby 3.3.0
```

Rails のアプリを WSL2 上で立ち上げる準備完了。

↓からは、実際にRailsのアプリをWSL2上で立ち上げてみる練習。

## 簡単なCRUDアプリ作成


### Step 1. 事前チェック
``` 
ruby -v          # 3.3.x が出ていればOK
gem -v
``` 
SQLiteを使うので、未インストールなら以下も

```
sudo apt update
sudo apt install -y libsqlite3-dev
```

### Step 2. 練習用アプリ作成
```
cd # Ubunts上の任意のディレクトリ
rails new sample_app -d sqlite3
cd Ruby_app
```

### Step 3. DB 準備 & サーバ起動
```
bin/rails db:prepare
bin/rails s -b 0.0.0.0
```
http://localhost:3000 を開き、Rails のウェルカムページが出たら成功。

### Step 4. 実際に動くもの（当リポジトリ初回コミットの内容）
```
bin/rails g scaffold Post title:string body:text
bin/rails db:migrate
bin/rails s -b 0.0.0.0
```

http://localhost:3000/posts で一覧/新規/編集/削除の確認ができれば完成。
