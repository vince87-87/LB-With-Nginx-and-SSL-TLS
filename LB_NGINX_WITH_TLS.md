# Load Balancer Solution With Nginx and SSL/TLS

# Configure Nginx As Load Balance

#Provision EC2 instance using ubuntu ami

![image](https://user-images.githubusercontent.com/49937302/119352481-50d29a00-bcd4-11eb-961d-da31a5a31a6a.png)

#configure /etc/hosts to resolve the hostname

172.31.26.234   web1
172.31.17.49    web2

![image](https://user-images.githubusercontent.com/49937302/119353195-2b925b80-bcd5-11eb-8615-7851dba43bd2.png)

#Update repo & install nginx

sudo apt update -y
sudo apt install nginx -y

![image](https://user-images.githubusercontent.com/49937302/119353745-dc005f80-bcd5-11eb-87a5-6658d0b47caa.png)

#Configure nginx

sudo vi /etc/nginx/nginx.conf

#insert the following config under http section & comment out

 upstream myproject {
    server Web1 weight=5;
    server Web2 weight=5;
  }

server {
    listen 80;
    server_name www.domain.com;
    location / {
      proxy_pass http://myproject;
    }
  }

#include /etc/nginx/sites-enabled/*;

![image](https://user-images.githubusercontent.com/49937302/119354848-1f0f0280-bcd7-11eb-9f31-f00c042a9ecc.png)

#restart nginx

sudo systemctl restart nginx
sudo systemctl status nginx

![image](https://user-images.githubusercontent.com/49937302/119356453-043d8d80-bcd9-11eb-9846-5b42bab910f9.png)

#verify that web page are load balance between 2 servers

![image](https://user-images.githubusercontent.com/49937302/119357885-9f833280-bcda-11eb-9300-294a8b981b18.png)

nginx

![image](https://user-images.githubusercontent.com/49937302/119357930-aad65e00-bcda-11eb-99e1-408dde59e9ce.png)

web1

sudo cat /var/log/httpd/access_log | tail

![image](https://user-images.githubusercontent.com/49937302/119358040-cccfe080-bcda-11eb-829a-091d4f698561.png)

web2

![image](https://user-images.githubusercontent.com/49937302/119358082-d78a7580-bcda-11eb-9fc4-389ef5277bb7.png)

# Register new domain & configure ssl/tls certificate

#register a domain at godaddy 

domain: mycloudslab.com

Assign a Elastic ip and associate to nginx server

Create A record on godaddy and point it to nginx server public ip

browse to http://mycloudslab.com , you should able to see tooling website

![image](https://user-images.githubusercontent.com/49937302/119362144-fbe85100-bcde-11eb-9452-06c93d0f39a8.png)

# Install certbot & request ssl/tls certificate



