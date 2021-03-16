# wordpress 설치


## 리눅스 버전 확인
```
  cat /etc/os-release
```

## apache2 설치
```
sudo apt install apache2
```
  - 설정
  ```
  sudo systemctl start apache2.service
  sudo systemctl enable apache2.service
  ```
touch /etc/apache2/sites-available/wordpress.conf
```
## mariaDB 설치
```
sudo apt install mariadb-server mariadb-client
sudo systemctl start mariadb.service
sudo systemctl enable mariadb.service
```
  - root password 설정
  ```
  sudo mysql_secure_installation
  ```
  ```
  Enter current password for root (enter for none): Just press Enter
  Set root password? [Y/n]: Y
  New password: Enter password
  Re-enter new password: Repeat password
  Remove anonymous users? [Y/n]: Y
  Disallow root login remotely? [Y/n]: Y
  Remove test database and access to it? [Y/n]: Y
  Reload privilege tables now? [Y/n]: Y
  ```
  - 워드프레스 데이터베이스 생성
  ```
  sudo mysql -u root -p
  ```
  ```
  CREATE DATABASE WP_database;
  CREATE USER 'wp_user'@localhost IDENTIFIED BY 'mypassword';
  GRANT ALL ON WP_database.* TO 'wp_user'@localhost IDENTIFIED BY 'mypassword' WITH GRANT OPTION;
  FLUSH PRIVILEGES;
  EXIT
  ```
## WordPress on Linux Mint 20 
  - wordpress 설치
  ```
  cd /tmp
  wget https://wordpress.org/latest.tar.gz
  tar -zxvf latest.tar.gz
  sudo mkdir -p /var/www/html/wordpress
  sudo mv wordpress /var/www/html/
  sudo chown -R www-data:www-data /var/www/html/
  sudo chgrp www-data /var/www/html/wordpress/
  sudo chmod g+w /var/www/html/wordpress/
  ```
  - wordpress 설정
  ```
  sudo mv /var/www/html/wordpress/wp-config-sample.php /var/www/html/wordpress/wp-config.php
  sudo vim /var/www/html/wordpress/wp-config.php
  ```
  ```
  define(‘DB_NAME’, ‘WP_database’);
  define(DB_USER’, ‘wp_user’);
  define(DB_PASSWORD’, ‘mypassword’);
  ```
  ```
  sudo vim /etc/nginx/sites-available/wordpress
  ```
  ```
  server {
	listen 80;
	listen [::]:80;
	server_name app.example.com  example.com
	root /var/www/html/wordpress;
	index index.php index.html index.htm;

	location / {
		try_files $uri $uri/ index.php?$args;
	}
	location ~ \.php$ {
		include snippets/fastcgi-php.conf;
		fastcgi_pass unix:/run/php/php7.4-fpm.sock;
		fastcgi_param	SCRIPT_FILENAME $document_root$fastcgi_script_name;
	  }
  }
  ```
  
  
