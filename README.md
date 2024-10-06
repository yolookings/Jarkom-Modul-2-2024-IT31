# Jarkom-Modul-2-2024-IT31

## Anggota Kelompok IT31 :

| Nama Lengkap         | NRP        |
| -------------------- | ---------- |
| Maulana Ahmad Zahiri | 5027231010 |
| Dzaky Faiq Fayyadhi  | 5027231047 |

## Laporan Resmi Modul 2 Jarkom - IT31

## Topologi Jaringan 1 - IT31

![alt text](img/topologi-1.jpg)

### Prefix IP IT31 : 192.232

### Konfig Nusantara :

```sh
// konfig Nusantara

auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
  address 192.232.1.1
  netmask 255.255.255.0

auto eth2
iface eth2 inet static
  address 192.232.2.1
  netmask 255.255.255.0

auto eth3
iface eth3 inet static
  address 192.232.3.1
  netmask 255.255.255.0

up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.232.0.0/16
```

## Switch1

```
// HayamWuruk

auto eth0
iface eth0 inet static
  address 192.232.1.2
  netmask 255.255.255.0
  gateway 192.232.1.1

up echo nameserver 192.168.122.1 > /etc/resolv.conf

// Sriwijaya

auto eth0
iface eth0 inet static
  address 192.232.1.3
  netmask 255.255.255.0
  gateway 192.232.1.1

up echo nameserver 192.168.122.1 > /etc/resolv.conf

// Tanjungkulai

auto eth0
iface eth0 inet static
  address 192.232.1.4
  netmask 255.255.255.0
  gateway 192.232.1.1

up echo nameserver 192.168.122.1 > /etc/resolv.conf

// Bedahulu

auto eth0
iface eth0 inet static
  address 192.232.1.5
  netmask 255.255.255.0
  gateway 192.232.1.1

up echo nameserver 192.168.122.1 > /etc/resolv.conf

// Kotalingga

auto eth0
iface eth0 niet static
  address 192.232.1.6
  netmask 255.255.255.0
  gateway 192.232.1.1

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

## Switch2

```
// Solok

auto eth0
iface eth0 inet static
  address 192.232.2.2
  netmask 255.255.255.0
  gateway 192.232.2.2

up echo nameserver 192.168.122.1 > /etc/resolv.conf

// Majapahit

auto eth0
iface eth0 inet static
  address 192.232.2.3
  netmask 255.255.255.0
  gateway 192.232.2.2

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

## Switch3

```
// GajahMada

auto eth0
iface eth0 inet static
  address 192.232.3.2
  netmask 255.255.255.0
  gateway 192.232.3.3

up echo nameserver 192.168.122.1 > /etc/resolv.conf

// ThomasAlfaEdison

auto eth0
iface eth0 inet static
  address 192.232.3.3
  netmask 255.255.255.0
  gateway 192.232.3.3

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

lalu setelah semua konfigurasi selesai bisa kita tes dengan masuk ke web consolenya dan menginputkan command untuk melakukan ping sebagaimana seperti berikut :

```
ping google.com
```

jika masih belum dapat melakukan ping, maka kita perlu untuk mengecek /etc/resolv.conf

```
nano /etc/resolv.conf
```

dan perhatikan bahwasanya direktori /etc/resolv.conf terisi dengan nameserver seperti berikut:

```
nameserver 192.168.122.1 > /etc/resolv.conf
```

lalu coba untuk lakukan reload dan jalankan kembali ping.

## Soal 1

Untuk mempersiapkan peperangan World War MMXXIV (Iya sebanyak itu), Sriwijaya membuat dua kotanya menjadi web server yaitu Tanjungkulai, dan Bedahulu, serta Sriwijaya sendiri akan menjadi DNS Master. Kemudian karena merasa terdesak, Majapahit memberikan bantuan dan menjadikan kerajaannya (Majapahit) menjadi DNS Slave.

berdasarkan topologi terlihat bahwasanya terdapat :

- 3 client
- 3 web server
- 2 DNS (yang terdiri dari Master dan Slave)
- 1 load balancer

lalu terdapat pembagian seperti ini :

- router > nusantara
- DNS Master > Sriwijaya
- DNS Slave > Majapahit

untuk menjadikan DNS server, lakukan instalasi bind (update dahulu sebelum mengisntal bind9)

```
 apt-get update
```

lalu install bind

```
 apt-get install bind9 -y
```

## Soal 2

Karena para pasukan membutuhkan koordinasi untuk melancarkan serangannya, maka buatlah sebuah domain yang mengarah ke Solok dengan alamat sudarsana.xxxx.com dengan alias www.sudarsana.xxxx.com, dimana xxxx merupakan kode kelompok. Contoh: sudarsana.it01.com.

IT31 > sudarsana.it31.com

Lakukan perintah pada Sriwijaya yang merupakan dns master

```
nano /etc/bind/named.conf.local
```

lalu sesuaikan sudarsana.it31.com dengan syntax berikut:

```
zone "sudarsana.it31.com" {
	type master;
	file "/etc/bind/it31/sudarsana.it31.com";
};
```

sehingga menjadi seperti pada gambar berikut :

![alt text](/img/name-conf.png)

lalu setelah itu membuat folder tempat sudarsana di /etc/bind :

```
mkdir /etc/bind/it31
```

dan copykan file db.local ke direktori tersebut :

```
cp /etc/bind/db.local /etc/bind/it31/sudarsana.it31.com
```

lalu edit dan arahkan ke ip solok > 192.232.2.2
![alt text](/img/sudarsana.png)

lalu restart bind9

```
service bind9 restart
```

![alt text](/img/restart-bind9.png)

## Soal 3

Para pasukan juga perlu mengetahui mana titik yang akan diserang, sehingga dibutuhkan domain lain yaitu pasopati.xxxx.com dengan alias www.pasopati.xxxx.com yang mengarah ke Kotalingga.

T31 > pasopati.it31.com | target > kotalingga

Lakukan perintah pada Sriwijaya yang merupakan dns master

```
nano /etc/bind/named.conf.local
```

lalu sesuaikan pasopati.it31.com dengan syntax berikut:

```
zone "pasopati.it31.com" {
	type master;
	file "/etc/bind/it31/pasopati.it31.com";
};
```

lalu setelah itu membuat folder tempat pasopati di /etc/bind :

```
mkdir /etc/bind/it31
```

dan copykan file db.local ke direktori tersebut :

```
cp /etc/bind/db.local /etc/bind/it31/pasopati.it31.com
```

lalu edit dan arahkan ke ip kotalingga > 192.232.1.6
![alt text](/img/pasopati.png)

lalu restart bind9

```
service bind9 restart
```

![alt text](/img/restart-bind9.png)

## Soal 4

Markas pusat meminta dibuatnya domain khusus untuk menaruh informasi persenjataan dan suplai yang tersebar. Informasi dan suplai meme terbaru tersebut mengarah ke Tanjungkulai dan domain yang ingin digunakan adalah rujapala.xxxx.com dengan alias www.rujapala.xxxx.com.

T31 > rujapala.it31.com | target > kotalingga

Lakukan perintah pada Sriwijaya yang merupakan dns master

```
nano /etc/bind/named.conf.local
```

lalu sesuaikan rujapala.it31.com dengan syntax berikut:

```
zone "rujapala.it31.com" {
	type master;
	file "/etc/bind/it31/rujapala.it31.com";
};
```

lalu setelah itu membuat folder tempat rujapala di /etc/bind :

```
mkdir /etc/bind/it31
```

dan copykan file db.local ke direktori tersebut :

```
cp /etc/bind/db.local /etc/bind/it31/rujapala.it31.com
```

lalu edit dan arahkan ke ip kotalingga > 192.232.1.6
![alt text](/img/rujapala.png)

lalu restart bind9

```
service bind9 restart
```

![alt text](/img/restart-bind9.png)

## Soal 5

Pastikan domain-domain tersebut dapat diakses oleh seluruh komputer (client) yang berada di Nusantara.

kita dapat mengecek dari komputer client, ambil contoh yakni pada client HayamWuruk, kita dapat mengecek nameserver terlebih dahulu

```
cat /etc/resolv.conf
```

lalu kita dapat menambahkan nameserver dari DNS master, yakni sriwijaya yang memiliki ip : 192.168.1.3.

```
echo nameserver 192.168.1.3 >> /etc/resolv.conf
```

![alt text](/img/cek-resolv.png)

lalu setelah itu kita dapat melakuakn ping kepada semua domain.

- untuk domain sudarsana

```
ping sudarsana.it31.com -c 4
```

- untuk domain pasopati

```
ping pasopati.it31.com -c 4
```

- untuk domain rujapala

```
ping rujapala.it31.com -c 4
```

sehingga akan menampilkan ping seperti berikut :

![alt text](/img/cek-ping.png)

## Soal 6

Beberapa daerah memiliki keterbatasan yang menyebabkan hanya dapat mengakses domain secara langsung melalui alamat IP domain tersebut. Karena daerah tersebut tidak diketahui secara spesifik, pastikan semua komputer (client) dapat mengakses domain pasopati.xxxx.com melalui alamat IP Kotalingga (Notes: menggunakan pointer record).

1. kita dapat menambahkan pointer record yang terhubung dengan ip kotalingga

```
zone "1.232.192.in-addr.arpa" {
  type slave;
  masters{192.232.1.3;};
  file "/var/lib/bind/1.232.192.in-addr.arpa"
}
```

2. lalu setelah itu kita dapat membuat dns record, dengan cara mengcopy db.local ke domain kita :

```
cp /etc/bind/db.local /etc/bind/it31/1.232.192.in-addr.arpa
```

3. lalu edit dns record nya

```

```

4. setelah itu restart bind9 nya

```
service bind9 restart
```

5. setelah itu test ptr dengan menjalankan host pada client
   ![alt text](/img/test-ping-ptr.png)

## Soal 7

Akhir-akhir ini seringkali terjadi serangan brainrot ke DNS Server Utama, sebagai tindakan antisipasi kamu diperintahkan untuk membuat DNS Slave di Majapahit untuk semua domain yang sudah dibuat sebelumnya yang mengarah ke Sriwijaya.

1. pada console sriwijaya kita dapat mengedit di bagian /etc/bind/named.conf.local

```
zone "sudarsana.it31.com" {
  type slave;
  masters{192.232.1.3;};
  file "/var/lib/bind/sudarsana.it31.com"
}

zone "pasopati.it31.com" {
  type slave;
  masters{192.232.1.3;};
  file "/var/lib/bind/pasopati.it31.com"
}

zone "rujapala.it31.com" {
  type slave;
  masters{192.232.1.3;};
  file "/var/lib/bind/rujapala.it31.com"
}

zone "1.232.192.in-addr.arpa" {
  type slave;
  masters{192.232.1.3;};
  file "/var/lib/bind/1.232.192.in-addr.arpa"
}
```

2. lakukan restart pada bin9

```
service bind9 restart
```

3. kemudian pastikan sudah ada nameserver dari DNS server dan DNS Slave pada client

4. kemudian pada console sriwijaya, matikan bind9 untuk kita melakukan pengetesan apakah bisa berjalan melalui DNS Slave

```
service bind9 stop
```

5. lalu setelah itu lakukan pengetesan ping pada client (terserah mau yang mana)

![alt text](/img/test-ping-slave.png)

6. lalu setelah itu nyalan kembali bind9 pada console sriwijaya

```
service bind9 start
```

## Soal 8

Kamu juga diperintahkan untuk membuat subdomain khusus melacak kekuatan tersembunyi di Ohio dengan subdomain cakra.sudarsana.xxxx.com yang mengarah ke Bedahulu.

1. masuk ke sriwijaya console, dan edit dns rec nya dengan menambahkan sub domain cakra di baris paling bawah

![alt text](/img/cakra.png)

2. lalu restart bind9 pada pada console sriwijaya

```
service bind9 restart
```

3. lalu masuk ke web consle client dan lakukan ping.cakra

![alt text](/img/cakra.png)

## Soal 9

Karena terjadi serangan DDOS oleh shikanoko nokonoko koshitantan (NUN), sehingga sistem komunikasinya terhalang. Untuk melindungi warga, kita diperlukan untuk membuat sistem peringatan dari siren man oleh Frekuensi Freak dan memasukkannya ke subdomain panah.pasopati.xxxx.com dalam folder panah dan pastikan dapat diakses secara mudah dengan menambahkan alias www.panah.pasopati.xxxx.com dan mendelegasikan subdomain tersebut ke Majapahit dengan alamat IP menuju radar di Kotalingga.

1. masuk ke console sriwijaya lalu pada `/etc/bind/it31/sudarsana.it31.com/` kita menggunakan domain pasopati dan menambahkan 2 baris yakni `www`dan`panah`lalu ,pada panah dan`ns1` buat ip yang menuju ke majapahit

![alt text](/img/panah.png)

2. edit konfigurasi pada `/etc/bind/named.conf.options` menjadi seperti berikut :

![alt text](/img/options-panah.png)

3. lalu lakukan restart pada bind

```
service bind9 restart
```

4. pada web console majapahit `/etc/bind/named.conf.options` , kita mengatur option dan juga `conf.local` nya

   `opt`
   ![alt text](/img/options-panah.png)
   `local`
   ![alt text](/img/zone-local-1.png)
   ![alt text](/img/zone-local-2.png)

5. lalu setelah itu buat direktori panah pada `/etc/bind/panah`, dan buat untuk menuju ke ip kotalingga

![alt text](/img/panah-kotalingga.png)

6. lakukan restart pada bind

```
service bind9 restart
```

7. lakukan tes ping pada subdomain panah
   ![alt text](/img/ping-panah.png)

## Soal 10

Markas juga meminta catatan kapan saja meme brain rot akan dijatuhkan, maka buatlah subdomain baru di subdomain panah yaitu log.panah.pasopati.xxxx.com serta aliasnya www.log.panah.pasopati.xxxx.com yang juga mengarah ke Kotalingga.

1. pada /etc/bind/named.conf.local tambahkan zone log.panah

![alt text](/img/log-panah-local.png)

2. cd majapahit `/etc/bind`, lalu tambahkan bagian subdomain `log`dan`www.log`

![alt text](/img/log-panah.png)

3. lakukan restart pada bind

```
service bind9 restart
```

4. kemudian lakukan ping pada `www.log` dan `log` it31.com

![alt text](/img/ping-log-panah.png)

## Soal 11

Setelah pertempuran mereda, warga IT dapat kembali mengakses jaringan luar dan menikmati meme brainrot terbaru, tetapi hanya warga Majapahit saja yang dapat mengakses jaringan luar secara langsung. Buatlah konfigurasi agar warga IT yang berada diluar Majapahit dapat mengakses jaringan luar melalui DNS Server Majapahit.

1. lakukan konfigurasi pada `/etc/bind/named.conf.options`

```
nano /etc/bind/named.conf.options
```

2. pada DNS Majapahit tambahkan dns forwarder

![alt text](/img/option-dns.png)

3. kemudian lakukan restart bind9

```
service bind9 restart
```

4. kemudian pada client lakukan tes ping untuk memeriksa sambungan internet mereka :
   ![alt text](/img/tes-client-internet.png)

## Soal 12

Karena pusat ingin sebuah laman web yang ingin digunakan untuk memantau kondisi kota lainnya maka deploy laman web ini (cek resource yg lb) pada Kotalingga menggunakan apache.

1. setup dan konfugarasi apache terlebih dahulu pada kotalingga :

```
apt-get install update && apt-get install apache2 libapache2-mod-php7.0 php wget unzip -y
```

dari konfigurasi diatas untuk dapat melakukan :

- install php, wget, unzip

dan jangan lupa untuk menginstall lynx sebagai tempat testing nya

```
apt-get install lynx
```

2. lalu kita dapat melakukan konfigurasi pada apache2 mengikuti ketentuan dari modul :

```
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/pasopati.it31.com.conf
```

3. kemudian lakukan config pada `/etc/apache2/sites-available/pasopati.it31.com.conf` sebagaimana sebagai gambar berikut:

![alt text](/img/conf-lynx.png)

4. setelah itu buat direktori untuk nantinya kira akan mengkonfigurasi file yang kita unduh pada soal shift

```
mkdir /var/www/pasopati.it31.com
```

5.  kemudian kita aktifkan (enable) konfigurasi web

```
a2ensite pasopati.it31.com.conf
```

6. lalu kita unduh file tersbeut ke direktori kita

```
wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1Sqf0TIiybYyUp5nyab4twy9svkgq8bi7' -O lb.zip
```

![alt text](/img/download-lb.png)

7. kemudian di ekstrak

```
unzip lb.zip -d lb
```

8. kemudian pindahkan folder ke direktori `/var/www/pasopati.it31.`

```
mv lb/\* /var/www/pasopati.it31.com
```

9. kemudian pindah kan isi dari file yang berupa `index.php` ke direktori kita:

```
cp /var/www/pasopati.it31.com/worker/index.php /var/www/pasopati.it31.com/index.php
```

9. kemudian pindahkan ke folder `/var/www/html` dan lakukan testing menggunakan lynx

```
cp /var/www/pasopati.it31.com/index.php /var/www/html/index.php
```

10. jika masih belum bisa melakukan testing, maka kita harus menghapus file htmlnya terlebih dahulu

```
rm /var/www/html/index.html
```

untuk melakuakn testing dapat dengan cara pastikan lynx sudah terinstal dan jalankan `http://192.232.1.6/index.php` (ini ke kotalingga)

![alt text](/img/testing-lynx.png)

## Soal 13

Karena Sriwijaya dan Majapahit memenangkan pertempuran ini dan memiliki banyak uang dari hasil penjarahan (sebanyak 35 juta, belum dipotong pajak) maka pusat meminta kita memasang load balancer untuk membagikan uangnya pada web nya, dengan Kotalingga, Bedahulu, Tanjungkulai sebagai worker dan Solok sebagai Load Balancer menggunakan apache sebagai web server nya dan load balancer nya.

1. kita harus melakukan setup terlebih dahulu pada solok

- dengan menggunakan apache2

```
apt-get update && apt-get install apache2 -y
```

2. untuk mengaktifkan konfigurasi ke sebuah konfigurasi apache2

```
a2enmod proxy
a2enmod proxy_balancer
a2enmod proxy_http
a2enmod lbmethod_byrequests
```

3. kemudian kita lakukan konfigurasi apache di `/etc/apache2/sites-available/000-default.conf`

![alt text](/img/conf-apache-13.png)

4. lalu kemudian restart apache2

```
service apache2 restart
```

5. Lalu ulangi semua langkah dan lakukan config pada setiap web sesuai dengan nomor [Soal 12](#Soal-12)

6. lakukan testing

![alt text](/img/testing-tanjungkulai.png)

![alt text](/img/testing-kotalingga.png)

## Soal 14

Selama melakukan penjarahan mereka melihat bagaimana web server luar negeri, hal ini membuat mereka iri, dengki, sirik dan ingin flexing sehingga meminta agar web server dan load balancer nya diubah menjadi nginx.

1. pada web serever kita perlu untuk menginstall nginx dan php-fpm yang nantinya akan mensupport lynx

```
apt-get install nginx php-fpm -y
```

2. kemudian kita edit file config sesuai domain dari web server

```
server {
    listen 80;

    root /var/www/pasopati.it31.com;

    index index.php index.html index.htm;
    server_name _ pasopati.it31.com www.pasopati.it31.com;

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
}
```

3. lalu masukkan file tersebut ke `/etc/nginx/sites-available/domain`

4. lalu clone ke direktori `sites-enabled` dan remove file default page nginx

```
ln -s /etc/nginx/sites-available/pasopati.it31.com /etc/nginx/sites-enabled/pasopati.it31.com
rm /etc/nginx/sites-enabled/default
```

5. kemudian lakukan restart

```
service nginx restart
service php7.0-fpm start
```

6. kemudian kita lakukan setting untuk load-balancer nya

- pada solok kira install `nginx`

  ```
  apt-get install nginx -y
  ```

- kemudian edit config nginx, padda `/etc/nginx/sites-available/solok`

  ```
    upstream webserver  {
        server 192.232.1.4;
        server 192.232.1.5;
        server 192.232.1.6;
    }

    server {
        listen 80;
        server_name _;

    location / {
        proxy_pass http://webserver;
    }
  }
  ```

- kemudian lakukan link dengan `sites-enabled` lalu hapus config default nginx

```
ln -s /etc/nginx/sites-available/solok /etc/nginx/sites-enabled/solok
rm /etc/nginx/sites-enabled/default
```

- terakhir lakukan restart pada nginx

```
service nginx restart
```

test load balancer

![alt text](/img/tanjungkulai.png)

![alt text](/img/kotalingga.png)

![alt text](/img/bedahulu.png)

![alt text](image.png)

## Soal 15

Markas pusat meminta laporan hasil benchmark dengan menggunakan apache benchmark dari load balancer dengan 2 web server yang berbeda tersebut dan meminta secara detail dengan ketentuan:

1. masuk ke web server solok

2. kemudian lakukan benchmark load balancer apache2 menggunakan byrequest jadikan default`(ab -n 100 -c 10 http://127.0.0.1/)`
   ![alt text](/img/byrequest.png)

3. lalu atur konfigurasi untuk semua algoritma yang dipakai

4. lalu cd `/etc/apache2/sites-available`
   ![alt text](/img/algo.png)

5. kemudian lakukan benchmark load balancer apache2 menggunakan bytraffic
   ![alt text](/img/bytraffic.png)
6. kemudian lakukan benchmark load balancer apache2 menggunakan bybusyness
   ![alt text](/img/bybusyness.png)

## Soal 16

Karena dirasa kurang aman dari brainrot karena masih memakai IP, markas ingin akses ke Solok memakai solok.xxxx.com dengan alias www.solok.xxxx.com (sesuai web server terbaik hasil analisis kalian). tanjungkulai

1. pada DNS Master lakukan konfigurasi pada `/etc/bind/named.conf.local`

```
zone "solok.it31.com" {
    type master;
    file "/etc/bind/it31/solok.it31.com";
};
```

![alt text](/img/config-16.png)

2. kemudiian edit dns record kemudina copy localnya yang domain awalnya sudarsana menjadi solok.it31.com dan menuju ke ip webserver `192.232.1.4`

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     solok.it31.com. root.solok.it31.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      solok.it31.com.
@       IN      A       10.67.1.4
@       IN      AAAA    ::1
www     IN      CNAME   solok.it31.com.
```

3. restart bind

```
service bind9 restart
```

4. kemudian pada console solok, `/etc/apache2/sites-available`, dan copy `000-default.conf` `solok.it31.com.conf` , dan janganlupa untuk mengganti servernam dan server alias nya

![alt text](/img/console-16.png)

5. lalu `nano apache2.conf` dan tambahkan servername solok.it31.com

![alt text](/img/apache2-solok.png)

6. kemudina nonaktifkan konfigurasi website yang aktif `a2dissite 000-default.com`

7. dan aktifkan konfigurasi untuk solok `a2ensite solok.it17.com`

8. lakukan restart apache2

```
service apache2 restart
```

9. kemudian masuk ke web console client menggunakan ip `majapahit` dan `sriwijaya`

![alt text](/img/console-web-16.png)

10. kemudian lakukan testing `lynx` pada ketiga web server

![alt text](/img/tanjungkulai.png)

![alt text](/img/kotalingga.png)

![alt text](/img/bedahulu.png)

## Soal 17

Agar aman, buatlah konfigurasi agar solok.xxx.com hanya dapat diakses melalui port sebesar π x 10^4 = (phi nya desimal) dan 2000 + 2000 log 10 (10) +700 - π = ?.

1. masuk ke web console solok, dan masuk ke `/etc/apache2`

2. kemudian `nano ports.conf`dan tambahkan line `Listen` 22686 dan 31415 (sesuai perhitungan soal)

![alt text](/img/listen.png)

3. rm /var/www/html/\*

4. kemudian edit konfigurasi virtualhostnya pada `nano sites-available` dan ganti sesuai dengna listen port nya yakni `22686 dan 31415`

![alt text](/img/vh.png)

5. lakukan restart apache2

```
service apache2 restart
```

6. kemudian pada client server lakuakn testing solok `lynx solok.it31.com`
   ![alt text](/img/lynx-solo.png)

7. lakukan testing lynx :

- `lynx solok.it17.com:22686`
  ![alt text](/img/kotalingga.png)

- `lynx solok.it17.com:31415`
  ![alt text](/img/bedahulu.png)

## Soal 18

Apa bila ada yang mencoba mengakses IP solok akan secara otomatis dialihkan ke www.solok.xxxx.com.

1. pada console solok masuk ke direktori `/etc/apache2/sites-available`, dan lakukan `nano redirect.conf`

```
<VirtualHost *:80>
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        RewriteEngine On
        RewriteRule ^(.*)$ http://solok.it17.com:4696/$1 [R=301,L]
</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```

2. kemudian aktifkan konfigurasi apache2 nya

```
a2enmod rewrite
```

3. kemudian aktifkan konfigurasi website nya

```
a2ensite redirect.conf
```

4. lakukan restart

```
service apache2 restart
```

5. kemudian pada web client
   ![alt text](/img/client.png)

6. kemudian lakuakn testing pada lynx dari IP solok `192.232.3.3`
   ![alt text](/img/kotalingga.png)

## Soal 19

Karena probset sudah kehabisan ide masuk ke salah satu worker buatkan akses direktori listing yang mengarah ke resource worker2.

1. lakukan edit `/etc/bind/named.conf.local` tambahkan zone baru untuk sekiantterimakasih

```
zone "sekiantterimakasih.it31.com" {
    type master;
    file "/etc/bind/it31/sekiantterimakasih.it31.com";
};
```

![alt text](/img/sekian.png)

2. lakukan konfigurasi di `/etc/bind/it31/sekiantterimakasih.it31.com` untuk ke ip kotalingga

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sekiantterimakasih.it31.com. root.sekiantterimakasih.it31.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      sekiantterimakasih.it31.com.
@       IN      A       10.67.1.5
www     IN      CNAME   sekiantterimakasih.it31.com.
```

3. lakukan restart bind

```
service bind9 restart
```

4. dan juga tambahkan server untuk domain sekiantterimakasih di `/etc/nginx/sites-available/cakra.sudarsana.it31.com`

```
server {
    listen 80;
    server_name sekiantterimakasih.it31.com www.sekiantterimakasih.it31.com;

    root /var/www/sekiantterimakasih.it31.com/dir-listing/worker2;
    index index.php index.html index.htm;

    location / {
        autoindex on;
        try_files $uri $uri/ =404;
    }
}
```

5. lalu untuk konfigurasi dirlistnya:

- download file nya:

```
wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1JGk8b-tZgzAOnDqTx5B3F9qN6AyNs7Zy' -O dir-listing.zip
```

- lalu lakukan unzip :

```
unzip dir-listing.zip -d dir-listing
```

- lalu buat direktori tempatnya :

```
mkdir /var/www/sekiantterimakasih.it31.com
```

- lalu pindahkan ke direktori `/var/www` :

```
mv dir-listing/* /var/www/sekiantterimakasih.it31.com
```

6. terakhir tinggal restart nginx nya :

```
service nginx restart
```

## Soal 20

Worker tersebut harus dapat di akses dengan sekiantterimakasih.xxxx.com dengan alias www.sekiantterimakasih.xxxx.com.

![alt text](/img/last-to-goal.png)
