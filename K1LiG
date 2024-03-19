#!/bin/bash

echo "欢迎使用此脚本！请选择一个选项："
echo "1. 更新软件源并安装组件"
echo "2. 安装 Docker Compose"
echo "3. 备份功能（此功能需要你自己实现）"
echo "4. AI网站搭建"
echo "5. 更新AI网站"
echo "6. 退出"
read -p "请输入你的选择（1-6）：" choice

case $choice in
1)
    echo "正在更新软件源并安装组件..."
    sudo apt-get update && sudo apt-get upgrade -y
    ;;
2)
    echo "正在安装 Docker Compose..."
    sudo apt-get install docker-compose -y
    ;;
3)
    echo "备份功能尚未实现。"
    ;;
4)
    echo "正在搭建 AI 网站..."
    read -p "请输入你的域名：" domain
    read -p "请输入你的端口：" port
    echo "
    version: '3'
    services:
      one-api:
        image: justsong/one-api
        restart: always
        ports:
          - $port:3000
        environment:
          - TZ=Asia/Shanghai
        volumes:
          - /home/ubuntu/data/one-api:/data
    " > docker-compose.yml
    docker-compose up -d
    echo "正在配置 Nginx..."
    echo "
    server{
       server_name $domain;
       
       location / {
              client_max_body_size  64m;
              proxy_http_version 1.1;
              proxy_pass http://localhost:$port;
              proxy_set_header Host $host;
              proxy_set_header X-Forwarded-For $remote_addr;
              proxy_cache_bypass $http_upgrade;
              proxy_set_header Accept-Encoding gzip;
              proxy_read_timeout 300s;
       }
    }" | sudo tee /etc/nginx/sites-available/default
    echo "正在配置 HTTPS..."
    sudo snap install --classic certbot
    sudo ln -s /snap/bin/certbot /usr/bin/certbot
    sudo certbot --nginx
    sudo service nginx restart
    ;;
5)
    echo "正在更新 AI 网站..."
    docker-compose down
    docker-compose pull
    docker-compose up -d
    ;;
6)
    echo "退出脚本。"
    exit 0
    ;;
*)
    echo "无效的选项。"
    ;;
esac
