# Laravel/Lumen 简单运行环境

## 使用方式
1. 复制 .env.example 到 .env，修改其中的环境变量
2. 添加站点。在修改的 NGINX 站点目录下添加对应的站点。如 NGINX_SITES_CONF_PATH=/var/www，那么就在 /var/www 下面添加 nginx 站点。在 sites-example 下复制对应的文件即可
3. 运行 `docker-compose up -d`
4. 访问测试是否运行成功
