# vagrant-chef-ubuntu-nginx-wordpress

* WordPressをUbuntu,Nginxで動かす環境をChefでプロビジョニングするVagrantfileです。
* 開発環境はVirtualBox, 本番環境はEC2インスタンス1つでの運用に対応。
* 既に運用しているWordPressのEC2への引っ越しにも使えます。

## インストール方法

### VirtualBoxインストール
 * https://www.virtualbox.org/

### Vagrantインストール
 * http://www.vagrantup.com/

### Vagrantプラグインインストール
```
vagrant plugin install vagrant-aws
vagrant plugin install vagrant-omnibus
vagrant plugin install vagrant-hostsupdater
```

### 初期設定
```
# wordpressフォルダをローカルに準備
cd wordpress
git clone https://github.com/takatoshi/vagrant-chef-ubuntu-nginx-wordpress.git vagrant
cd vagrant

# 環境依存ファイルを準備
cp vb/Vagrantfile.Sample vb/Vagrantfile
cp aws/Vagrantfile.Sample aws/Vagrantfile
cp vb/wordpress_import.sh.Sample vb/wordpress_import.sh
cp aws/wordpress_export.sh.Sample aws/wordpress_export.sh

# 開発環境のサーバー、WordPress、MySQL情報を入力
vi vb/Vagrantfile
# 本番環境のEC2、サーバー、WordPress、MySQL情報を入力
vi aws/Vagrantfile

# 本番環境のサーバー接続設定、MySQL情報を入力
vi vb/wordrepss_import.sh
# 開発環境のサーバー接続設定、MySQL情報を入力
vi aws/wordpress_export.sh
```
## VirtualBoxで開発

### 初回
```
cd vb
# 既に他サーバーでWordPressを使っている場合、
# そこのWordPressのDBをダンプ、templateに格納
./wordpress_import.sh
vagrant up
```

### 初回以降
#### vm起動
```
cd vb
vagrant up
```

#### vm停止
```
cd vb
vagrant halt
```

#### vm除去
```
cd vb
vagrant destroy
```

#### 本番環境のDB同期
```
cd vb
./wordpress_import.sh
vagrant up
vagrant provision
```

## EC2にデプロイ
### 初回
```
cd aws
# 開発環境のwordpressのDBをダンプ、templateに格納
./wordpress_export.sh
vagrant up
```

### 初回以降
```
# 開発環境でWordPressのアップデート、プラグインインストール、テーマ編集など
cd aws
./wordpress_export.sh
vagrant provision
```
