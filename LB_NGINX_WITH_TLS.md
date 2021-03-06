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

![image](https://user-images.githubusercontent.com/49937302/119416817-11816900-bd27-11eb-97cd-0f9d5644156e.png)

Create A record on godaddy and point it to nginx server public ip

![image](https://user-images.githubusercontent.com/49937302/119420999-447c2a80-bd30-11eb-9a63-e06a599e8160.png)

browse to http://toolings.mycloudslab.com , you should able to see tooling website

![image](https://user-images.githubusercontent.com/49937302/119420988-3b8b5900-bd30-11eb-8ee7-85fb784f8a29.png)

# Install certbot & request ssl/tls certificate

Install certbot and request for an SSL/TLS certificate

#update nginx configuration 

sudo vi /etc/nginx/nginx.conf
sudo systemctl restart nginx

![image](https://user-images.githubusercontent.com/49937302/119421122-92912e00-bd30-11eb-834e-8c8ca17d0a3b.png)

#install certbot

sudo snap install --classic certbot

#Ensure certbot can be run

sudo ln -s /snap/bin/certbot /usr/bin/certbot

#have Certbot edit your Nginx configuration automatically to serve it, turning on HTTPS access in a single step.

sudo certbot --nginx

![image](https://user-images.githubusercontent.com/49937302/119420894-fe26cb80-bd2f-11eb-82c1-939f2e1fe543.png)

Verify https://toolings.mycloudslab.com & https is accessible with ssl cert

![image](https://user-images.githubusercontent.com/49937302/119422126-ebfa5c80-bd32-11eb-8ba9-c4c7a3f407f0.png)

#Test automatic renewal

sudo certbot renew --dry-run

![image](https://user-images.githubusercontent.com/49937302/119581257-c9307c80-bdf4-11eb-8785-cd0f747f8f18.png)

#Configure Cron job to automatically renew cert @ 1207am everyday

crontab -e

Add following line:

07 00  */1 * *   root /usr/bin/certbot renew > /dev/null 2>&1

![image](https://user-images.githubusercontent.com/49937302/119583627-9341c700-bdf9-11eb-8c63-6f6921b82b9a.png)

#verify cron is running

![image](https://user-images.githubusercontent.com/49937302/119583719-c421fc00-bdf9-11eb-9c08-dfe3337ba9ec.png)


