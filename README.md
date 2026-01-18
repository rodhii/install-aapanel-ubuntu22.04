# Install aapanel Ubuntu22.04

```
# update ubuntu
sudo apt-get update

# install curl jika belum ada
sudo apt install curl

#install aapanel
sudo -s
URL=https://www.aapanel.com/script/install_7.0_en.sh && if [ -f /usr/bin/curl ];then curl -ksSO "$URL" ;else wget --no-check-certificate -O install_7.0_en.sh "$URL";fi;bash install_7.0_en.sh ipssl
```

# install mssql pada php 5.6 di aapanel

```
# Install FreeTDS, karena mssql belum ada untuk php 5.6
cd /tmp
wget http://ibiblio.org/pub/Linux/ALPHA/freetds/stable/freetds-stable.tgz
tar zxvf freetds-stable.tgz
cd freetds-0.91
./configure --prefix=/usr/local/freetds --with-tdsver=8.0 --enable-msdblib --enable-dbmfix
make && make install
2. Install php mssql extension

# Here php56 is used to demonstrate
cd /tmp
wget http://cn2.php.net/distributions/php-5.6.30.tar.gz
tar -zxvf php-5.6.30.tar.gz
cd php-5.6.30/ext/mssql/
/www/server/php/56/bin/phpize
./configure --with-php-config=/www/server/php/56/bin/php-config --with-mssql=/usr
make && make install
```
tambahkan di php.ini
```
extension = mssql
```

# install pdo_mssql pada php 5.6 di aapanel, karena tidak ada versi untuk pdo untuk php 5.6 maka install pdo_mdlib

```
# Masuk folder temporary
cd /tmp

# Download source PHP 5.6.30 (kalau belum ada)
wget http://cn2.php.net/distributions/php-5.6.30.tar.gz

# Extract
tar -zxvf php-5.6.30.tar.gz

# Masuk folder ekstensi PDO DBLIB
cd php-5.6.30/ext/pdo_dblib

# Jalankan phpize untuk PHP 5.6 mu
/www/server/php/56/bin/phpize

# Configure ekstensi pdo_dblib dengan PHP 5.6 + path FreeTDS
./configure --with-php-config=/www/server/php/56/bin/php-config --with-pdo-dblib=/usr

# Compile & install
make && make install
```
