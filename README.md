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
   * 8 GiB Storage

Instance Reserve Proxy
   * 1 vCPUs
   * 1 GiB Memory
   * 8 GiB Storage

```json
[
    {
        "Instance": "i-0e4879ae9d642386a",
        "Nama": "Web Service",
        "privateip": "172.31.43.134",
        "publicip": "52.77.211.247"
    },
    {
        "Instance": "i-0ac6f314402af1de6",
        "Nama": "Reverse Proxy",
        "privateip": "172.31.39.10",
        "publicip": "54.254.255.142"
    }
]
```

Instalasi:

Launch Instance EC2
```sh
aws ec2 run-instances --image-id ami-082b1f4237bd816a1 --instance-type t2.micro --key-name erpan --subnet-id subnet-09ef7060c82762b9e --count 2
```

Beri nama/tags instance

[![](https://i.imgur.com/NNttgjj.png)]

**Instance Web Service**
1. Install Apache pada Instance Web Service
```sh
sudo apt install apache2
```
2. Edit listen port menjadi 8000
```sh
sudo nano /etc/apache2/ports.conf
``` 
```plaintext
# If you just change the port or add more ports here, you will likely also
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf

#Listen 80
Listen 8000
```

**Instance Reserve Proxy**
1. Install Nginx pada Instance Reserve Proxy
```sh
sudo apt install nginx
```
