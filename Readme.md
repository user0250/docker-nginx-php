# 使用docker安装 nginx  php-fpm

##使用方法
 ```

docker-compose up -d  #后台静默开启
docker-compose down #关闭服务

php 里面内置有composer  使用方法为 
1. docker exec -i -t (你php的容器名字) /bin/bash 进入你的容器里面使用
2. 占时没学会


conf 目录 配置目录  自己看
log 运行时候，log文件
www 项目目录 php 项目目录

```


docker-compose.yml 注释

```dockerfile

version: "2"
services:
  nginx:  #服务名字
    restart: always
    image: nginx #镜像
    ports: #开启映射端口
      - "80:80"
      - "443:443"
    volumes:  #文件映射
      - ./conf/nginx/conf.d:/etc/nginx/conf.d
      - ./conf/nginx/nginx.conf:/etc/nginx/nginx.conf
      - ./log/nginx:/var/log/nginx
    links: # 网络链接 php-fpm服务
      - "php-fpm:php-fpm"
    depends_on: #依赖，等php-cpm启动后，启动
      - php-fpm
    volumes_from: #磁盘共享
      - php-fpm
  php-fpm:
    restart: always
    image: bitnami/php-fpm
    expose:
      - "9000"
    volumes:
      - ./conf/php/php.ini:/opt/bitnami/php/etc/php.ini
      - ./conf/php/php-fpm.conf:/opt/bitnami/php/etc/php-fpm.conf
      - ./log/php:/opt/bitnami/php/logs
      - ./www:/app

```


