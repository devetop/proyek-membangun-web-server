<<<<<<< HEAD
dicoding
=======
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
3. Edit virtualhost ke port 8000
```sh
sudo nano /etc/apache2/sites-available/000-default.conf
```
```plaintext
<VirtualHost *:8000>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html
....
```
4. Restart service apache
```sh
sudo systemctl restart apache2
```
5. Cek listen port dengan perintah `ss`
```sh
ss -plntu  | grep -i 8000
```
```plaintext
tcp   LISTEN 0      511                     *:8000            *:*
```


**Instance Reserve Proxy**
1. Install Nginx pada Instance Reserve Proxy
```sh
sudo apt install nginx
```
2. Disable default virtualhost
```sh
sudo unlink /etc/nginx/sites-enabled/default
```
3. Buat dan edit file reverse-proxy.conf
```sh
sudo nano /etc/nginx/conf.d/reverse-proxy.conf
```
```plaintext
# Pada bagian proxy_pass sesuaikan dengan privateip dan port
server {
    listen 80;
    location / {
        proxy_pass http://172.31.43.134:8000;
    }
}
```
4. Restart service nginx
```sh
sudo systemctl restart nginx
```
5. tambahkan port 80 pada Inbound rules di security groups
![](https://imgur.com/0JNzjEo.png)
6. Test akses dengan menggunakan Ip public pada Instance Reverse Proxy
![](https://imgur.com/DXNaZjN.png)
7. Untuk menambahkan Limit access maximum client, Edit file reverse-proxy.conf
```sh
sudo nano /etc/nginx/conf.d/reverse-proxy.conf
```
```plaintext
limit_conn_zone $binary_remote_addr zone=limitconnbyaddr:20m;
limit_conn_status 429;

server {
    listen 80;
# limit menjadi 50 client
        limit_conn   limitconnbyaddr  50;
    location / {
        proxy_pass http://172.31.43.134:8000;
    }
}
```
8. Restart service nginx
```sh
sudo systemctl restart nginx
```
>>>>>>> 3248528e63ce7feaaa370c87a194c3f55ef71b46
