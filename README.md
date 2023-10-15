# Jarkom-Modul-2-E29-2023
## Anggota
Kelompok E29
| Nama | NRP |
| --- | --- |
| Sony Hermawan | 5025211226 |
| Sekar Ambar Arum | 5025211041 |

## Topologi 
## Config
## Soal 1
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian sebagai berikut. Folder topologi dapat diakses pada drive berikut 

## Script
```
ping google.com -c 5
```
### Hasil
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/57ce1aa1-0cb5-4d0b-9ced-d68e246757cc)
#### Sadewa
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/62bfcb02-d8f9-44d7-89bb-5ac7c843a1a7)
#### Nakula
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/8faf779c-d69b-4075-bf25-ccc36ccc9eea)

## Soal 2
Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.

### Script
#### Yudhistira
```
apt-get update
apt-get install bind9 -y

echo '
zone "arjuna.e29.com" {
        type master;
        file "/etc/bind/jarkom/arjuna.e29.com";
};
' >> /etc/bind/named.conf.local

cp /etc/bind/db.local /etc/bind/jarkom/arjuna.e29.com

file_path="/etc/bind/jarkom/arjuna.e29.com"
sed -i 's/localhost/arjuna.e29.com/g' $file_path
sed -i 's/127.0.0.1/10.51.4.2/g' $file_path

sed -i 's/127\.0\.0\.1/10.51.4.2/g' /etc/bind/jarkom/arjuna.e29.com
echo "www IN CNAME arjuna.e29.com." >> /etc/bind/jarkom/arjuna.e29.com

service bind9 restart
```
Menggunakan sed untuk mengganti isi dari template db.local. Mengganti localhost, ip, dan menambahkan CNAME www

#### Sadewa
```
echo 'nameserver 10.51.1.2' > /etc/resolv.conf
ping arjuna.e29.com -c 5
ping www.arjuna.e29.com -c 5
```
### Hasil
#### Sadewa
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/b944fd54-25f5-4017-bf3c-565af428cfdd)

## Soal 3
Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.

### Script
#### Yudhistira
```
apt-get update
apt-get install bind9 -y

echo '
zone "arjuna.e29.com" {
        type master;
        file "/etc/bind/jarkom/arjuna.e29.com";
};
' >> /etc/bind/named.conf.local

cp /etc/bind/db.local /etc/bind/jarkom/arjuna.e29.com

file_path="/etc/bind/jarkom/arjuna.e29.com"
sed -i 's/localhost/arjuna.e29.com/g' $file_path
sed -i 's/127.0.0.1/10.51.4.2/g' $file_path

sed -i 's/127\.0\.0\.1/10.51.4.2/g' /etc/bind/jarkom/arjuna.e29.com
echo "www IN CNAME arjuna.e29.com." >> /etc/bind/jarkom/arjuna.e29.com

service bind9 restart
```
Menggunakan sed untuk mengganti isi dari template db.local, mengganti localhost, ip, dan menambahkan CNAME www
#### Sadewa
```
echo 'nameserver 10.51.1.2' > /etc/resolv.conf
ping abimanyu.e29.com -c 5
ping www.abimanyu.e29.com -c 5
```
### Hasil
#### Sadewa
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/acd50678-ddbf-46ae-8ef7-7636415fd72c)

## Soal 4
Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

### Script
#### Yudhistira
```
sed -i 's/127\.0\.0\.1/10.51.1.2/g' /etc/bind/jarkom/abimanyu.e29.com
echo "parikesit IN A 10.51.1.2" >> /etc/bind/jarkom/abimanyu.e29.com
service bind9 restart
```
Menggunakan command sed untuk menambahkan subdomain parikesit pada abimanyu
#### Sadewa
```
ping parikesit.abimanyu.e29.com -c 5
```
### Hasil
#### Sadewa
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/6ce2da5e-aea7-4635-a8e6-ed737affdde4)

## Soal 5
Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)

### Script
#### Yudhistira
```
echo -e "zone \"4.51.10.in-addr.arpa\" {\n\ttype master;\n\tfile \"/etc/bind/jarkom/4.51.10.in-addr.arpa\";\n};\n" >> /etc/bind/named.conf.local
cp /etc/bind/db.local /etc/bind/jarkom/4.51.10.in-addr.arpa
sed -i 's/localhost/abimanyu.e29.com/g' /etc/bind/jarkom/4.51.10.in-addr.arpa
sed -i ':a;N;$!ba;s/@/4.51.10.in-addr.arpa./2' /etc/bind/jarkom/4.51.10.in-addr.arpa
echo "4 IN PTR abimanyu.e29.com." >> /etc/bind/jarkom/4.51.10.in-addr.arpa
service bind9 restart
```
Menambahkan line pada named.conf.local dan 4.51.10.in-addr.arpa menggunakan echo dan sed untuk melakukan reverse DNS
#### Sadewa
```
echo "nameserver 192.168.122.1" > /etc/resolv.conf

apt-get update
apt-get install dnsutils

echo "nameserver 10.51.1.2" > /etc/resolv.conf
echo "nameserver 10.51.4.4" >> /etc/resolv.conf

host -t PTR 10.51.4.4
```
### Hasil
#### Sadewa
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/2828bb33-a0bb-4eed-9be5-48aa79c16eee)

## Soal 6
Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

### Script
#### Yudhistira
```
line='type master;'
text='\tnotify yes;\n\talso-notify { 10.51.2.2; };\n\tallow-transfer { 10.51.2.2; };'

sed -i "/$line/a $text" /etc/bind/named.conf.local
```
Nenambahkan $text setelah line 'type master;' 
#### Werkudara
```
apt-get update
apt-get install bind9 -y

text='zone "abimanyu.e29.com" {
    type slave;
    masters { 10.51.1.2; };
    file "/var/lib/bind/abimanyu.e29.com";
};'

echo "$text" >/etc/bind/named.conf.local
service bind9 restart
```
Melakukan instalasi bind dan menambahkan slave pada werkudara
#### Yudhistira
```
service bind9 stop
```
Melakukan service stop untuk mengecek jika slave berfungsi
#### Sadewa
```
echo "nameserver 10.51.1.2" > /etc/resolv.conf
echo "nameserver 10.51.2.2" >> /etc/resolv.conf
ping abimanyu.e29.com -c 5
```
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/5bd91fc7-56a3-41c8-ac5d-137ae9b5c68e)

## Soal 7
Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.
#### Yudhistira
```
text='ns1 IN A 10.51.2.2\nbaratayuda IN NS ns1'
echo -e "$text" > /etc/bind/jarkom/abimanyu.e29.com 

sed -i 's/dnssec-validation auto;/# dnssec-validation auto;/g' /etc/bind/named.conf.options
sed -i '/# dnssec-validation auto;/a allow-query{any;};' /etc/bind/named.conf.options

sed -i '/notify yes;/d' /etc/bind/named.conf.local
sed -i '/also-notify { 10.51.2.2; };/d' /etc/bind/named.conf.local

service bind9 restart
```
Menambahkan ns1 pada, comment dnssec-validation auto; dan menambahkan allow-query{any;};. Melakukan delegasi subdomain ke ip 10.51.2.2 (Werkudara) menggunakan sed
#### Werkudara
```
sed -i 's/dnssec-validation auto;/# dnssec-validation auto;/g' /etc/bind/named.conf.options
sed -i '/# dnssec-validation auto;/a allow-query{any;};' /etc/bind/named.conf.options

sed -i 's/abimanyu.e29.com/baratayuda.abimanyu.e29.com/g' /etc/bind/named.conf.local
sed -i 's/type slave;/type master;/g' /etc/bind/named.conf.local
sed -i '/masters { 10.51.1.2; };/d' /etc/bind/named.conf.local
sed -i '/file "\/var\/lib\/bind\/baratayuda.abimanyu.e29.com";/d' /etc/bind/named.conf.local
sed -i '/type master;/a file "/etc/bind/delegasi/baratayuda.abimanyu.e29.com";' /etc/bind/named.conf.local

mkdir /etc/bind/delegasi
cp /etc/bind/db.local /etc/bind/delegasi/baratayuda.abimanyu.e29.com

sed -i 's/localhost/baratayuda.abimanyu.e29.com/g' /etc/bind/delegasi/baratayuda.abimanyu.e29.com
sed -i 's/127.0.0.1/10.51.4.4/g' /etc/bind/delegasi/baratayuda.abimanyu.e29.com
echo "www IN A 10.51.2.2" >> /etc/bind/delegasi/baratayuda.abimanyu.e29.com

service bind9 restart
```
Comment dnssec-validation auto; dan menambahkan allow-query{any;};, menambahkan slave dan menghapus master pada named.conf.local membuat file delegasi dan mengganti template pada dblocal
#### Sadewa
```
echo '
nameserver 10.51.1.2
nameserver 10.51.2.2
' >> /etc/resolv.conf
ping baratayuda.abimanyu.e29.com -c 5
ping www.baratayuda.abimanyu.e29.com -c 5
```

### Hasil
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/502a7fcd-aea9-428a-8c0c-d4ac34212395)

## Soal 8
Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.
### Script
#### Abimanyu
```
apt-get update
apt-get install bind9 -y

echo -e "zone \"abimanyu.e29.com\" {\n\ttype master;\n\tfile \"/etc/bind/jarkom/abimanyu.e29.com\";\n};\n" >> /etc/bind/named.conf.local

mkdir /etc/bind/jarkom
cp /etc/bind/db.local /etc/bind/jarkom/abimanyu.e29.com
sed -i 's/localhost/abimanyu.e29.com/g' /etc/bind/jarkom/abimanyu.e29.com
service bind9 restart

echo 'baratayuda IN A 10.51.2.2' >> /etc/bind/jarkom/abimanyu.e29.com
echo 'ns IN A 10.51.2.2' >> /etc/bind/jarkom/abimanyu.e29.com
echo 'rjp IN NS ns1' >> /etc/bind/jarkom/abimanyu.e29.com

sed -i 's/dnssec-validation auto;/# dnssec-validation auto;/g' /etc/bind/named.conf.options
sed -i '/# dnssec-validation auto;/a allow-query{any;};' /etc/bind/named.conf.options

sed -i '/file "\/etc\/bind\/jarkom\/abimanyu.e29.com";/a allow-transfer { 10.51.2.2; };' /etc/bind/named.conf.local

service bind9 restart
```
Melakukan setup ulang bind pada dan memberikan delegasi pada 10.51.2.2 (Werkudara)
#### Werkudara
```
echo 'www IN CNAME baratayuda.abimanyu.e29.com.' >> /etc/bind/delegasi/baratayuda.abimanyu.e29.com
sed -i '/www IN A 10.51.2.2/d' /etc/bind/delegasi/baratayuda.abimanyu.e29.com
echo 'www.rjp IN CNAME baratayuda.abimanyu.e29.com.' >> /etc/bind/delegasi/baratayuda.abimanyu.e29.com
echo 'rjp IN A 10.51.2.2' >> /etc/bind/delegasi/baratayuda.abimanyu.e29.com

service bind9 restart
```
Menambahkan CNAME, menambahkan www.rjp cname pada /etc/bind/delegasi/baratayuda.abimanyu.e29.com
#### Sadewa
```
echo 'nameserver 10.51.2.2' > /etc/resolv.conf
echo 'nameserver 10.51.4.4' >> /etc/resolv.conf
ping rjp.baratayuda.abimanyu.e29.com -c 5
ping www.rjp.baratayuda.abimanyu.e29.com -c 5
```
### Hasil
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/f17f5ff6-26f6-4a4c-abe3-60b5f07f6652)

## Soal 9
Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.
### Script
#### Arjuna
```
apt-get install bind9 nginx
apt-get install nginx
service nginx start

touch /etc/nginx/sites-available/lb-jarkom

echo '
 # Default menggunakan Round Robin
 upstream myweb  {
        server 10.51.4.3;
        server 10.51.4.4;
    server 10.51.4.5;
 }

 server {
        listen 80;
        server_name arjuna.e29.com;

        location / {
        proxy_pass http://myweb;
        }
 }
 ' >/etc/nginx/sites-available/lb-jarkom
ln -s /etc/nginx/sites-available/lb-jarkom /etc/nginx/sites-enabled

service nginx restart
nginx -t
```
Melakukan instalasi nginx dan setup LB pada Arjuna
#### Prabukusuma, Abimanyu, Wissanggeni 
```
apt-get update && apt install nginx php php-fpm -y
php -v
mkdir /var/www/jarkom
touch /var/www/jarkom/index.php
echo '
<?php
echo "Halo, Kamu berada di Prabukusuma"; //sesuai node
?>
' >>/var/www/jarkom/index.php
touch /etc/nginx/sites-available/jarkom
echo '
server {

        listen 80;

        root /var/www/jarkom;

        index index.php index.html index.htm;
        server_name _;

        location / {
                        try_files $uri $uri/ /index.php?$query_string;
        }

        # pass PHP scripts to FastCGI server
        location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
        }
 location ~ /\.ht {
                        deny all;
        }

        error_log /var/log/nginx/jarkom_error.log;
        access_log /var/log/nginx/jarkom_access.log;
}
' >>/etc/nginx/sites-available/jarkom
service php7.0-fpm start
rm -rf /etc/nginx/sites-enabled/default
ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled
service nginx restart
nginx -t
```
Server worker melakukan deployment
### Sadewa
```
echo 'nameserver 10.51.4.2
        nameserver 10.51.1.2
' > /etc/resolv.conf
lynx http://arjuna.e29.com
```
### Kendala
File index.php yang didownload melalui gdrive tidak bisa berjalan dan mengarahkan ke halaman default php

### Hasil
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/61f484a1-2ea1-4b22-b4e3-d9269bb3710c)
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/9ff92ae8-c372-42c5-8446-9dc7da465929)
## Soal 10
Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003
### Script
#### Arjuna
```
file_path="/etc/nginx/sites-available/lb-jarkom"

sed -i 's/server 10.51.4.3/server 10.51.4.3:8001/g' $file_path
sed -i 's/server 10.51.4.4/server 10.51.4.4:8002/g' $file_path
sed -i 's/server 10.51.4.5/server 10.51.4.5:8003/g' $file_path
```
Mengubah port pada file lb-jarkom
#### Prabukusuma, Abimanyu, Wissanggeni 
```
file_path="/etc/nginx/sites-available/jarkom"

sed -i 's/listen 80/listen 8001/g' $file_path // Sesuai node
```
#### Sadewa
Mengubah masing" port pada worker
### Hasil
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/61f484a1-2ea1-4b22-b4e3-d9269bb3710c)
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/9ff92ae8-c372-42c5-8446-9dc7da465929)

## Soal 11-12
11. Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy
12. Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.

### Script
#### Abimanyu
```
apt-get install apache2
service apache2 start
apt-get install php
php -v

cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/abimanyu.e29.com.conf

echo '

<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/abimanyu.e29.com
        ServerName abimanyu.e29.com
        ServerAlias www.abimanyu.e29.com

        <Directory /var/www/abimanyu.e29.com/index.php/home>
                Options +Indexes
        </Directory>
        # Soal 12
        Alias "/home" "/var/www/abimanyu.e29.com/index.php/home"


        ErrorLog ${APACHE_LOG_DIR}/error.log

        CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
# vim: syntax=apache ts=4 sw=4 sts=4 sr noet

' >/etc/apache2/sites-available/abimanyu.e29.com.conf

apt-get install libapache2-mod-php7.0
service apache2 restart
service nginx stop
```
Mendownload file dari drive
```
apt-get install wget
wget -O abimanyu.zip "https://drive.google.com/uc?export=download&id=1a4V23hwK9S7hQEDEcv9FL14UkkrHc-Zc"
apt install unzip
mkdir temp
unzip abimanyu.zip -d temp
mkdir /var/www/abimanyu.e29.com
cp temp/abimanyu.yyy.com/index.php /var/www/abimanyu.e29.com/index.php
cp temp/abimanyu.yyy.com/home.html /var/www/abimanyu.e29.com/home.html
cp temp/abimanyu.yyy.com/abimanyu.webp /var/www/abimanyu.e29.com/abimanyu.webp
a2ensite abimanyu.e29.com
service apache2 restart
```
#### Sadewa
```
// Soal 11
lynx http://abimanyu.e29.com
lynx http://www.abimanyu.e29.com
// Soal 12
lynx www.abimanyu.e29.com/home
```
### Hasil
http://abimanyu.e29.com
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/e098925f-087b-49a3-a2a0-978f0e64f450)
http://www.abimanyu.e29.com
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/a9f57434-4b8a-40fb-bc48-d8d4f774b93f)
www.abimanyu.e29.com/home
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/c932a951-d628-4e95-8772-ad9785a67852)

## Soal 13-16 dan 20
13. Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy
14. Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).
15. Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.
16. Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi 
www.parikesit.abimanyu.yyy.com/js 

### Script
#### Abimanyu
```
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/parikesit.abimanyu.e29.com.conf

a2enmod rewrite
service apache2 restart

echo '

<VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/parikesit.abimanyu.e29.com
        ServerName parikesit.abimanyu.e29.com
        ServerAlias www.parikesit.abimanyu.e29.com

        # Soal 20
        RewriteEngine On
        RewriteCond %{REQUEST_URI} abimanyu
        RewriteRule .* /public/images/abimanyu.png [L]
```
-Indexes pada directory secret agar file didalamnya tidak dapat diakses
```
        # Soal 14
        <Directory /var/www/parikesit.abimanyu.e29.com/public>
                Options +Indexes
        </Directory>

        <Directory /var/www/parikesit.abimanyu.e29.com/secret>
                Options -Indexes
        </Directory>

        <Directory /var/www/parikesit.abimanyu.e29.com/public/js>
                Options +Indexes
        </Directory>
```
Menggunakan alias untuk menyingkat url
```
        # Soal 16
        Alias "/js" "/var/www/abimanyu.e29.com/public/js"
```
Kustomisasi file page 404 menggnakan ErrorDocument diarahkan ke file 404.html dan 403.html yang didownload
```
        # Soal 15
        ErrorDocument 404 /error/404.html
        ErrorDocument 403 /error/403.html
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
' >/etc/apache2/sites-available/parikesit.abimanyu.e29.com.conf
```
Mendownload file menggunakan wget dan mengunzipnya
```
wget -O parikesit.zip "https://drive.google.com/uc?export=download&id=1LdbYntiYVF_NVNgJis1GLCLPEGyIOreS"
unzip parikesit.zip -d /var/www/
mv /var/www/parikesit.abimanyu.yyy.com /var/www/parikesit.abimanyu.e29.com
```
Pembuatan file" pada /secret
```
mkdir /var/www/parikesit.abimanyu.e29.com/secret
touch /var/www/parikesit.abimanyu.e29.com/secret/index.html

echo
'
<html
  style="
    height: 100;
    background-repeat: no-repeat;
    background-size: 100% 100%;
    height: 100%;
    width: 100%;
  "
>
  <header>
    <h1 style="text-align: center; color: white">
      Sussy Baka
    </h1>
  </header>
</html>
' >>/var/www/parikesit.abimanyu.e29.com/secret/index.html

touch /var/www/parikesit.abimanyu.e29.com/secret/amogus.html

echo
'
<html
  style="
    height: 100;
    background-repeat: no-repeat;
    background-size: 100% 100%;
    height: 100%;
    width: 100%;
  "
>
  <header>
    <h1 style="text-align: center; color: white">
      Amogus
    </h1>
  </header>
</html>
' >>/var/www/parikesit.abimanyu.e29.com/secret/amogus.html

a2ensite parikesit.abimanyu.e29.com
service apache2 restart                                   
```
#### Yudhistira
```
echo "www.parikesit IN CNAME abimanyu.e29.com." >> /etc/bind/jarkom/abimanyu.e29.com
sed -i '/www IN CNAME abimanyu.e29.com./d' /etc/bind/jarkom/abimanyu.e29.com
service bind9 restart
```
Mengganti cname www menjadi www.parikesit
#### Sadewa
```
// Soal 13
lynx http://parikesit.abimanyu.e29.com
// Soal 14
lynx http://www.parikesit.abimanyu.e29.com/secret
// Soal 15
lynx http://www.parikesit.abimanyu.e29.com/232
// Soal 16
lynx http://www.parikesit.abimanyu.e29.com/js
```
### Hasil
#### Soal 13
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/5cc637fd-a9db-45a2-8a4f-9d304cabbf43)
#### Soal 14
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/04ae26ed-b967-402e-a715-08c5a6f2e65b)
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/88dd7b5c-2ea1-42f1-95c3-5c603e22a12c)
#### Soal 15
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/480434c2-94ec-4701-923d-f0d0ac270482)
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/46b2fafc-e66f-4cd3-a020-875853f9c7b9)
#### Soal 16
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/0320c354-a784-4470-a73e-f4c25d85af67)
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/0acf40ac-524c-42c9-afc1-ea17a2a1d9cf)

## Soal 17-18
17. Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.
18. Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.
### Script
#### Abimanyu
```
wget -O rjp.baratayuda.zip "https://drive.google.com/uc?export=download&id=1pPSP7yIR05JhSFG67RVzgkb-VcW9vQO6"
unzip rjp.baratayuda.zip -d /var/www/
mv /var/www/rjp.baratayuda.abimanyu.yyy.com /var/www/rjp.baratayuda.abimanyu.e29.com

touch /etc/apache2/sites-available/rjp.baratayuda.abimanyu.e29.com.conf

echo '
<VirtualHost *:14000>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/rjp.baratayuda.abimanyu.e29.com
        ServerName rjp.baratayuda.abimanyu.e29.com
        ServerAlias www.rjp.baratayuda.abimanyu.e29.com
```
Fungsi Untuk membuat sistem auth
```
        # Soal 18
        <Directory "/var/www/rjp.baratayuda.abimanyu.e29.com">
           Options Indexes FollowSymLinks
           AllowOverride None
           Require all granted
        </Directory>

        <Location />
           AuthType Basic
           AuthName "Restricted Content"
           AuthUserFile /etc/apache2/.htpasswd
           Require valid-user
        </Location>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
Virtual host untuk IP 14400
```
<VirtualHost *:14400>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/rjp.baratayuda.abimanyu.e29.com
        ServerName rjp.baratayuda.abimanyu.e29.com
        ServerAlias www.rjp.baratayuda.abimanyu.e29.com

        # Soal 18
          <Directory "/var/www/rjp.baratayuda.abimanyu.e29.com">
           Options Indexes FollowSymLinks
           AllowOverride None
           Require all granted
       </Directory>

       <Location />
           AuthType Basic
           AuthName "Restricted Content"
           AuthUserFile /etc/apache2/.htpasswd
           Require valid-user
       </Location>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
' >>/etc/apache2/sites-available/rjp.baratayuda.abimanyu.e29.com.conf
```
Menggenerate username dan password
```
username="Wayang"
password="baratayudae29"

# Define the path to the .htpasswd file
htpasswd_file="/etc/apache2/.htpasswd"

# Use htpasswd to create the .htpasswd file with the specified username and password
htpasswd -b -c $htpasswd_file $username $password
```
Menambahkan port yang bisa digunakan
```
echo '
Listen 14000
Listen 14400
' >>/etc/apache2/ports.conf

a2enmod rewrite
a2ensite rjp.baratayuda.abimanyu.e29.com
service apache2 restart
```
#### Sadewa
```
lynx http://10.51.4.4:14000
lynx http://10.51.4.4:14400
// Pengecekan port salah
lynx http://10.51.4.4:14500
```
### Hasil
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/b3f75702-f811-4c88-a853-067ef5f84f4b)
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/50c127bb-9e15-4e68-a40d-857dde4bd300)
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/903a4885-ec49-490a-b5a5-35f1557c39cf)
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/9214f64b-bf04-4515-9845-12ee6498d0a7)


## Soal 19
Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)
### Script
#### Abimanyu
```
service apache2 restart
echo '
<VirtualHost *:80>
        # Soal 19
        ServerName 10.51.4.4

        RewriteEngine On
        RewriteCond %{REMOTE_ADDR} ^10\.51\.4\.4$
        RewriteRule ^/(.*) http://www.abimanyu.e29.com/\$1 [R=301,L]


        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/abimanyu.e29.com
        #ServerName abimanyu.e29.com
        ServerAlias www.abimanyu.e29.com


        <Directory /var/www/abimanyu.e29.com/index.php/home>
                Options +Indexes
        </Directory>
        
        Alias "/home" "/var/www/abimanyu.e29.com/index.php/home"
        
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
' >/etc/apache2/sites-available/abimanyu.e29.com.conf

service apache2 restart
```
#### Sadewa
```
lynx http://10.51.4.4
```
### Hasil
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/56f16faf-7de1-4afc-97bc-6b9dfc5627c3)


## Soal 20
Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.
### Script
#### Abimanyu
Di soal 16
```
      # Soal 20
        RewriteEngine On
        RewriteCond %{REQUEST_URI} abimanyu
        RewriteRule .* /public/images/abimanyu.png [L]
```
Melakukan rewrite pada file jika ada substring abimanyu menjadi abimanyu.png
#### Sadewa
```
lynx http://www.parikesit.abimanyu.e29.com/public/images
```
### Hasil
![image](https://github.com/AdonisZK/Jarkom-Modul-2-E29-2023/assets/48209612/986809c3-0389-4d7d-8f14-d3158615885d)


