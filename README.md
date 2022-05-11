# About-LEMP
## การติดตั้ง LEMP โดยมีการติดตั้งโปรแกรมต่อไปนี้

L->Linux (ในทีนี้ใน Git bash) ,E->Nginx ,M->MariaDB ,P->PHP

## การ Config docker-compose สำหรับใช้งาน Nginx 
สร้าง Folder ชื่อ nginx_dock ภายใน folder จะประกอบไปด้วย
    
* nginx_dock
    * docker-compose.yml
    * static-html
        * index.html

ข้อมูลภายใน file ชื่อ docker-compose.yml
```
version: '3'

services:
  nginx:
    container_name: nginx
    restart: unless-stopped
    image: nginx:stable-alpine
    volumes:
      - ./static-html:/usr/share/nginx/html
    ports:
      - "81:80"

networks:
  default:
    external:
      name:
        web_network

```

## การ Config Nginx และ php FPM Container
(สร้างแยกจาก Folder ก่อนหน้าต่างหาก)

เริ่มต้นสร้าง Folder ชื่อ lemp_dock ภายใน folder จะประกอบไปด้วย 

* lemp_dock
    * docker-compose.yml
    * html
        * index.php
    * nginx
        * conf
            > nginx.conf
        * conf.d
            > default.conf

