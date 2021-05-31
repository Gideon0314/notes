---
title: Docker
created: '2021-04-25T08:08:13.489Z'
modified: '2021-05-31T09:44:13.110Z'
---

## Docker

#### Flask 项目使用部署步骤
1. git pull 更新代码
2. 构建镜像
3. 删除历史镜像，暂停移除正运行的容器
4. 启动新构建的镜像


#### 删除名称为<none>的容器
```
docker image prune -f
```
