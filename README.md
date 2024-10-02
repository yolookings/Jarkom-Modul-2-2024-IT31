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
