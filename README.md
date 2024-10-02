# Jarkom-Modul-2-2024-IT31

## Anggota Kelompok IT31 :

| Nama Lengkap         | NRP        |
| -------------------- | ---------- |
| Maulana Ahmad Zahiri | 5027231010 |
| Dzaky Faiq Fayyadhi  | 5027231047 |

## Write Up

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
