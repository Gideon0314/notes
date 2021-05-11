---
title: Jenkins
created: '2021-04-25T08:57:26.697Z'
modified: '2021-05-06T05:43:58.484Z'
---

# Jenkins

构建完成后 command 使用CURL请求接口
```
curl -H "Content-Type:application/json" -X POST -d "{\"project_id\":\"d209b620-8c88-4f5a-a435-cab1bf74195a\",\"git_branch\":\"$GIT_BRANCH\"}" http://gideon.fun:5000/api/build_history
```

