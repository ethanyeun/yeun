---
title: initcontainerds（初始化容器）
date: 2024-08-29 10:52:30
tags: k8s
password: 1
message: '请输入密码查看内容'
hint: '生日'
categories: 
  - dev
  - k8s
---

> - 在pod中可以有一个或者多个初始化容器
> - 初始化容器执行成功后，应用的容器才能被执行
> - 初始化容器是仅运行一次的任务
> - 可以完成与预先指定的任务,阻止或者延迟容器启动

<!-- more -->

```
apiVersion: v1
kind: Pod
metadata:
  name: initnginx
spec:
  initContainers:
  - name: install
    image: busybox
    command:
    - wget
    - "-O"
    - "/work-dir/index.html"
    - "https://www.baidu.com"
    volumeMounts:
    - name: workdir
      mountPath: /work-dir
  containers:
  - name: nginx
    image: 172.20.45.174:81/base/nginx:1.15-alpine
    ports:
    - containerPort: 80
    volumeMounts:
    - name: workdir
      mountPath: /usr/share/nginx/html
  dnsPolicy: Default
  volumes:
  - name: workdir
    emptyDir: {}
```

```
kubectl get pods -w
```

