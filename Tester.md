---
title: Tester
created: '2021-04-21T02:05:31.386Z'
modified: '2021-05-11T08:20:46.759Z'
---

## Tester

#### 项目前端部署
```cmd
[root@CentOS www]# cd tester_web
[root@CentOS tester_web]# cnpm install
[root@CentOS tester_web]# npm run build
[root@CentOS tester_web]# mkdir -p ../docker/nginx/data
[root@CentOS tester_web]# cp -a dist/* ../docker/nginx/data
[root@CentOS tester_web]# cd ..
[root@CentOS www]# 
```
