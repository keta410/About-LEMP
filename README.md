# About-LEMP
## การติดตั้ง LEMP โดยมีการติดตั้งโปรแกรมต่อไปนี้

L->Linux (ในทีนี้ใน Git bash) ,E->Nginx ,M->MariaDB ,P->PHP

ก่อนทำการติดตั้งโปรแกรมในลำดับถัดไป
ให้สร้าง Network ด้วยคำสั่ง ```docker network create web_network```

## คำสั่งที่เกี่ยวข้องในการสร้าง Folder และ File
```
  cd <name folder>       // สร้าง folder นั้น
  cd ..                 //ย้อนกลับมายัง folder ก่อนหน้า
  touch <name file>     //สร้าง file .นามสกุลที่เราต้องการ
  rm <name folder>     //ลบ file หรือ folder นั้น
  ls                  //ดูข้อมูลต่างๆ ที่มีอยู่ใน folder นั้นๆ
  pwd                //ดู path ของ file หรือ folder นั้นๆ
```

## การ Config docker-compose สำหรับใช้งาน **Nginx** 
สร้าง Folder ชื่อ nginx_dock ภายใน folder จะประกอบไปด้วย
    
* nginx_dock
    * docker-compose.yml
    * static-html
        * index.html
(มีการใส่ข้อมูลภายใน file docker-compose.yml ของ nginx_dock)

จากนั้นทำการสั่ง ```docker-compose up -d``` ก็เป็นอันเสร็จสิ้น และทำเช่นเดียวกันในการติดตั้งโปรแกรมถัดไป

## การ Config Nginx และ **PHP**
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

File docker-compose.yml ของ lemp_dock
```
version: '3'

services:
  php:
    container_name: lemp_php
    image: php:7.4-fpm-alpine
    restart: unless-stopped
    volumes:
      - ./html/:/var/www/html
    expose:
      - "9000"

  nginx:
    container_name: lemp_nginx
    image: nginx:stable-alpine
    restart: unless-stopped
    volumes:
      - ./html/:/var/www/html

      - ./nginx/conf/nginx.conf:/etc/nginx/conf/nginx.conf:ro
      - ./nginx/conf.d:/etc/nginx/conf.d:ro

    ports:
      - "80:80"

networks:
  default:
    external:
      name:
        web_network
```

File inex.php ใน html
```
<?php phpinfo();?>
```

File nginx.conf | |

File default.conf | |

## การ Config **MariaDB**
สร้าง Folder และ File เพิ่มเติมใน lemp_dock ดังนี้

* lemp_dock
    * docker-compose.yml
    * html
        * index.php
    * nginx
        * conf
            > nginx.conf
        * conf.d
            > default.conf
    * mariadb
        * data
        * initdb 
            > titanic.sql           
    * php
        * Dockfile

แก้ไขข้อมูลภายใน File docker-compose.yml ของ lemp_dock | |

แก้ไขข้อมูลภายใน File index.php | |

File Dockerfile| |


สำหรับ File titanic.sql ในที่นี้ ดาวน์โหลดมาจาก>(https://github.com/pitimon/xOps_Summer/tree/master/20220507/LEMP/mariadb/initdb)

เมื่อจัดการเกี่ยว File ต่างๆ เรียบร้อย ทำการสั่ง ```docker-compose build``` ตามด้วย ```docker-compose up -d```

สำหรับในทุกขั้นตอนหลังติดตั้งโปรแกรมเสร็จ สามารถทำการตรวจสอบได้ตาม URL ที่เรากำหนด
