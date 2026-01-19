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

# Menginstal Microsoft ODBC Driver 17 for SQL Server di Ubuntu 22.04 
### yang menggunakan aaPanel memerlukan akses SSH (root) karena driver perlu diinstal pada tingkat sistem operasi, bukan melalui GUI aaPanel. 

## Langkah 1: Akses SSH dan Update Sistem 
- Masuk ke terminal server Anda melalui SSH sebagai root.
  Update daftar paket sistem:
```
sudo apt update
sudo apt upgrade -y
```
 
## Langkah 2: Tambahkan Repositori Microsoft 
### Ubuntu 22.04 membutuhkan repositori Microsoft agar bisa mengunduh msodbcsql17. 
Instal dependensi yang diperlukan:
```
sudo apt install apt-transport-https gnupg2 unixodbc-dev -y
```

### Tambahkan GPG key Microsoft:
```
curl https://packages.microsoft.com/keys/microsoft.asc | sudo gpg --dearmor -o /usr/share/keyrings/microsoft-prod.gpg
```

### Tambahkan repositori prod untuk Ubuntu 22.04:
```
curl https://packages.microsoft.com/config/ubuntu/22.04/prod.list | sudo tee /etc/apt/sources.list.d/mssql-release.list
```

### Update kembali daftar paket:
```
sudo apt update
```

## Langkah 3: Instal ODBC Driver 17 
Jalankan perintah berikut untuk menginstal driver ODBC 17 dan menyetujui EULA (End User License Agreement): 
```
sudo ACCEPT_EULA=Y apt-get install -y msodbcsql17
```

## Langkah 4: Verifikasi Instalasi 
Pastikan driver telah terinstal dengan benar:
```
odbcinst -q -d -n "ODBC Driver 17 for SQL Server"
```

### Jika terinstal, akan muncul informasi driver.

##Langkah 5: Hubungkan ke PHP di aaPanel 
###  Agar situs Anda di aaPanel bisa menggunakan driver ini, Anda harus menginstal ekstensi PHP sqlsrv dan pdo_sqlsrv. 
- Buka aaPanel > App Store.
- Cari versi PHP yang Anda gunakan (misal: PHP 8.2), lalu klik Settings.
- Pilih tab Install Extensions.
- Cari dan klik "Install" pada sqlsrv dan pdo_sqlsrv.
- Restart layanan PHP Anda setelah instalasi selesai. 

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
