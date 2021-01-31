# Cara dasar melakukan pengaturan server
berikut adalah cara paling dasar bagaimana mengatur server dimana pengaturan di sini merupakan pengaturan yang diperlukan ketika membuat cluster kubernetes. Untuk tutorial ini saya menggunakan Ubuntu Server 20.04

Untuk mempermudah login sebagai super user dahulu:
```sh
sudo su
```
# Mengubah hostname
cara ubah hostname:
```sh
#edit hostname kemudian ubah menjadi nama yang mudah dikenali, dan hanya gunakan angka dan huruf tanpa spasi 
nano /etc/hostname
```
# Mengubah IP address
cara mengubah IP:
```sh
#edit configuration, atau buat file bila belum ada
nano /etc/netplan/00-installer-config.yaml

#ubah isi sesuai pengaturan IP
network:
  ethernets:
    enp0s3:
      addresses:
      - 192.168.100.41/24
      gateway4: 192.168.100.1
      nameservers:
        addresses:
        - 8.8.8.8
  version: 2

#apply pengaturan
netplan apply
```
# Mengubah timezone
cek dahulu timezone server sekarang:
```sh
timedatectl

#cari daftar timezone yang tersedia
timedatectl list-timezones
```
ubah timezone sesuai dengan daftar yang tersedia
```sh
sudo timedatectl set-timezone Asia/Jakarta
```
# Resize LVM
apabila storage server menggunakan sistem LVM maka gunakan seluruh storage LVM untuk penggunaan server
```sh
# Periksa apakah storage sudah terpakai semua
df -h
# Lihat bagian dev/mapper/ubuntu--vg-ubuntu--lv

lvm
lvm> lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
lvm> exit
resize2fs /dev/ubuntu-vg/ubuntu-lv
```
# Reboot Server
jangan lupa reboot server untuk memastikan pengaturan terbaru terpasang
```sh
#reboot server untuk apply pengaturan
reboot now
```
