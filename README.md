# Jarkom-Modul-2-E29-2023

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
echo "parikesit IN A 10.51.4.4" >> /etc/bind/jarkom/abimanyu.e29.com
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
### Script
### Hasil

## Soal 9
### Script
### Hasil

## Soal 10
### Script
### Hasil

## Soal 11
### Script
### Hasil

## Soal 12
### Script
### Hasil

## Soal 13
### Script
### Hasil

## Soal 14
### Script
### Hasil

## Soal 15
### Script
### Hasil

## Soal 16
### Script
### Hasil

## Soal 17
### Script
### Hasil

## Soal 18
### Script
### Hasil

## Soal 19
### Script
### Hasil

## Soal 20
### Script
### Hasil
