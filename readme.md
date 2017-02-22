# LEMP環境
vagrant + ansible + docker を使ったLEMP環境

## 必要環境
- virtualbox
- vagrant 1.8以上
    - vbguest plugin

## docker コンテナ一覧
- nginx stable (1.10)
- php-fpm 5.6
    - postfix メール送信可能
- mysql 5.6
- memcached latest (1.4)
- redis latest (3.2)

## dockerマシンの構成
[Vagrantfile](Vagrantfile)

- CPU: 2core
- memory: 4G
- IP: 192.168.33.11
- CentOS 7.2
- php 5.6
- mysql 5.6 クライアント
- redis-cli latest (3.2)
- Git latest (2.10)

## 利用方法

### vbguest pluginインストール
` vagrant plugin install vagrant-vbguest`

### VM起動
` vagrant up `  
このリポジトリをcloneしてディレクトリ配下で実行

### VM再起動
` vagrant reload `  
初回立ち上げ後のみ必要   
standaloneでansibleを実行するためOS再起動が出来ないのでselinux無効化などを反映させる必要がある

### dockerコンテナ起動
` sudo docker-compose -f /vagrant/docker/docker-compose.yml up -d`

## volumesディレクトリ
/volumesディレクトリの以下のディレクトリ、ファイルが各dockerコンテナにマウントされている  

### 全体構成
vagrantホームディレクトリ /home/vagrant/volumes -> /volumesにシンボリックリンクが張られている

        /volumes
        ├── configs
        │   ├── mysql
        │   │   └── custom.cnf # /etc/mysql/conf.d/custom.cnf
        │   ├── nginx
        │   │   └── conf.d # /etc/nginx/conf.d
        │   │       └── default.conf
        │   ├── php
        │   │   └── php.ini # /usr/local/etc/php/conf.d/php.ini
        │   └── redis
        │       └── redis.conf # /usr/local/etc/redis/redis.conf
        ├── logs
        │   ├── mail # /var/log/mail php-fpm上のメールログ
        │   └── nginx # /var/log/nginx
        └── workspace # nginxドキュメントルート

## 動作確認

### nginx + php-fpm
http://192.168.33.11/

PHPは/volumes/workspace配下にphpファイルを置くことで確認できる

### mysql
` mysql -u root -p -h 192.168.33.11 `

### redis
` redis-cli -h 192.168.33.11 `

### memcached
` telnet 192.168.33.11 11211 `


## docker関連

### mysql rootパスワード指定
/vagrant/docker/docker-compose.yml  
**MYSQL_ROOT_PASSWORD**にて指定


### memcached起動オプション指定
/vagrant/docker/docker-compose.yml  
memcachedコンテナの**command**オプションで起動オプションを指定
