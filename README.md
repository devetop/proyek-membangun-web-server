# Submission Proyek Membangun Web Server dengan Amazon EC2

Deskripsi: 
Menyiapkan 2 Instance Amazon EC2 dengan subnet-id yang sama. 
Instance pertama dengan nama Web Service dan instance kedua dengan nama Reserve Proxy.
Install apache pada instance Web Service
install nginx pada instance Reserve Proxy lalu disetting agar dapat meneruskan request client ke Instance Web Service.
Setting limit access pada instance Reserve Proxy

Teknologi yang digunakan:
* 2 Instance Amazon EC2 dengan instance type t2.micro 
* OS Ubuntu Server 22.04 LTS

Detail Spesifikasi:
Instance Web Service 
    * 1 vCPUs
    * 1 GiB Memory 
    * GiB Storage

Instance Reserve Proxy
    * 1 vCPUs
    * 1 GiB Memory 
    * GiB Storage

Instalasi:

1. Launch Instance EC2
```sh

```
