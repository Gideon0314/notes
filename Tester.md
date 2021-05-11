---
title: Tester
created: '2021-04-21T02:05:31.386Z'
modified: '2021-05-11T09:31:47.649Z'
---

## Tester

### 项目前端部署
```cmd
[root@CentOS www]# cd tester_web
[root@CentOS tester_web]# cnpm install
[root@CentOS tester_web]# npm run build
[root@CentOS tester_web]# mkdir -p ../docker/nginx/data
[root@CentOS tester_web]# cp -a dist/* ../docker/nginx/data
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
