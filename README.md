# WebRTC導入手順

## aws linux HTTPSサーバーの導入
### sslモジュールインストール
$ sudo yum install mod24_ssl<br>
$ httpd -M | grep ssl

### 秘密鍵作成
$ openssl genrsa > server.key

### CSR作成
$ openssl req -new -key server.key > server.csr

### サーバー証明書作成
$ openssl x509 -req -signkey server.key < server.csr >　server.crt<br>
$ rm server.csr

### 秘密鍵&サーバー証明書配置
$ sudo mkdir /etc/httpd/conf/ssl.key<br>
$ sudo mkdir /etc/httpd/conf/ssl.crt<br>
$ sudo mv server.key /etc/httpd/conf/ssl.key/<br>
$ sudo mv server.crt /etc/httpd/conf/ssl.crt/

### ssl.conf編集 
sudo vi /etc/httpd/conf.d/ssl.conf

!# SSLCertificateFile /etc/pki/tls/certs/localhost.crt<br>                             
SSLCertificateFile /etc/httpd/conf/ssl.crt/server.crt <br>
!# SSLCertificateKeyFile /etc/pki/tls/private/localhost.key<br>                        
SSLCertificateKeyFile /etc/httpd/conf/ssl.key/server.key   

### apache 再起動
$ sudo service httpd restart

### composerでhttps用のphp buildin server libraryインストール
$ curl -sS https://getcomposer.org/installer | php<br>
$ php composer.phar require --dev mpyw/php-hyper-builtin-server:^2.0

### httpsサーバー起動
$ vendor/bin/hyper-run -s 192.168.33.10:8000
