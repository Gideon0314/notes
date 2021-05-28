---
title: Tester
created: '2021-04-21T02:05:31.386Z'
modified: '2021-05-17T01:56:53.223Z'
---

## Tester

### 项目前端部署
```cmd
[root@CentOS www]# cd tester_web
[root@CentOS tester_web]# npm install
[root@CentOS tester_web]# npm run build:prod
[root@CentOS tester_web]# mkdir -p /usr/local/nginx/html
[root@CentOS tester_web]# cp -a dist/* /usr/local/nginx/html
[root@CentOS tester_web]# cd ..
[root@CentOS www]# 
```
### 项目后端部署
#### docker build
```
[root@CentOS www]# docker build -f tester_api/Dockerfile -t tester_api:0.0.1 .
```
#### docker run
```
[root@CentOS www]# docker run -d \
  --name tester_api \
  --link mysql:mysql-server \
  -e DATABASE_URL=mysql+pymysql://testuser:password@mysql-server/madblog \
  -p 5000:5000 \
  --rm \
  tester_api:0.0.1
```
