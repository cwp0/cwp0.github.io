---
title: k8s部署云笔记系统
tags:
  - Kubernetes
  - 教程
categories:
  - Kubernetes
description: 在k8s服务器上部署云笔记系统
top_img: ../image/k8s部署云笔记系统/可莉.png
abbrlink: 16330
date: 2023-09-19 22:35:00
updated: 2023-09-20 13:35:00
keywords:
comments:
cover:
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:
abcjs:
---

> 起因是导师之前让两个研一的学弟学妹写了个前后端的云笔记系统，然后后面让我部署到服务器中，因此记录一下。

## 创建namespace

```kubernetes helm
# 创建namespace
kubectl create namespace note
```

## 1. 部署MySQL

创建mysql文件夹

```shell
sudo mkdir -p /home/mysql/data
```

### 1.1 挂载数据卷

note-mysql-pvc.yaml

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: note-mysql-pv
spec:
  capacity:
	storage: 1Gi
  accessModes:
	- ReadWriteOnce
  hostPath:
	path: /home/mysql/data # 主机上的数据存储路径
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: note-mysql-pvc
spec:
  accessModes:
	- ReadWriteOnce
  resources:
	requests:
	  storage: 1Gi
  storageClassName: ""
	volumeName: note-mysql-pv
```

部署

```kubernetes helm
kubectl create -f note-mysql-pvc.yaml -n note
```

查看是否部署成功：

![mysql是否部署成功](../image/k8s部署云笔记系统/1.png)

### 1.2 部署mysql

note-mysql.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: note-mysql-deployment
spec:
  replicas: 1
  selector:
	matchLabels:
	  app: note-mysql-pod
  template:
	metadata:
	  labels:
		app: note-mysql-pod
	spec:
	  containers:
		- name: note-mysql
		  image: registry.cn-hangzhou.aliyuncs.com/lucas-njfu/note_mysql:v1.1.0
		  imagePullPolicy: IfNotPresent
		  env:
			- name: MYSQL_ROOT_PASSWORD
			  value: dfa6f1b032ee42ed95d12647225f2ff4
		  ports:
			- containerPort: 3306
		  volumeMounts:
			- name: note-mysql-persistent-storage
			  mountPath: /var/lib/mysql
	volumes:
	  - name: note-mysql-persistent-storage
		persistentVolumeClaim:
		  claimName: note-mysql-pvc # 使用之前创建的持久卷声明
---
apiVersion: v1
kind: Service
metadata:
  name: note-mysql-service
spec:
  selector:
	app: note-mysql-pod
  type: NodePort # service类型
  ports:
  - port: 3306
	nodePort: 30006 # 指定绑定的node的端口(默认的取值范围是：30000-32767), 如果不指定，会默认分配
	targetPort: 3306
```

部署：

```kubernetes helm
kubectl create -f note-mysql.yaml -n note
```

## 2. 部署redis

note-redis.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: note-redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: note-redis-pod
  template:
    metadata:
      labels:
        app: note-redis-pod
    spec:
      containers:
        - name: note-redis
          image: redis:latest
          imagePullPolicy: IfNotPresent
          env:
            - name: REDIS_PASSWORD
              value: adcc64a4a9b04324914626590fb52a1a
          ports:
            - containerPort: 6379
---
apiVersion: v1
kind: Service
metadata:
  name: note-redis-service
spec:
  selector:
    app: note-redis-pod
  type: NodePort # service类型
  ports:
  - port: 6379
    nodePort: 30079 # 指定绑定的node的端口(默认的取值范围是：30000-32767),如果不指定，会默认分配
    targetPort: 6379
```

部署：

```kubernetes helm
kubectl create -f note-redis.yaml -n note
```

## 3. 部署mongodb

创建mongodb文件夹

```shell
sudo mkdir -p /home/mongodb/data
```

### 3.1 挂载数据卷

note-mongodb-pvc.yaml

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: note-mongodb-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /home/mongodb/data # 主机上的数据存储路径
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: note-mongodb-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: ""
  volumeName: note-mongodb-pv
```

使用

```kubernetes helm
kubectl create -f note-mongodb-pvc.yaml -n note
```

查看是否装好：

![mongodb-pvc](../image/k8s部署云笔记系统/2.png)

### 3.2 部署

note-mongodb.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: note-mongodb-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: note-mongodb-pod
  template:
    metadata:
      labels:
        app: note-mongodb-pod
    spec:
      containers:
        - name: note-mongodb
          image: registry.cn-hangzhou.aliyuncs.com/lucas-njfu/note-mongodb:v1.1.0
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 27017
          env:
            - name: MONGO_INITDB_ROOT_USERNAME
              value: admin
            - name: MONGO_INITDB_ROOT_PASSWORD
              value: 5c8b18289f7848e9b0af98e81562649d
          volumeMounts:
            - name: note-mongodb-persistent-storage
              mountPath: /data/db
      volumes:
        - name: note-mongodb-persistent-storage
          persistentVolumeClaim:
            claimName: note-mongodb-pvc
---
apiVersion: v1
kind: Service
metadata:
  name: note-mongodb-service
spec:
  selector:
    app: note-mongodb-pod
  type: NodePort # service类型
  ports:
  - port: 27017
    nodePort: 30017 # 指定绑定的node的端口(默认的取值范围是：30000-32767),如果不指定，会默认分配
    targetPort: 27017
```

```kubernetes helm
kubectl create -f note-mongodb.yaml -n note
```

检查部署情况

![mongodb](../image/k8s部署云笔记系统/3.png)

## 4. 部署服务bert_flask

bert_flask.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: bert-flask-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: bert-flask-pod
  template:
    metadata:
      labels:
        app: bert-flask-pod
    spec:
      containers:
      - name: bert-flask
        image: registry.cn-hangzhou.aliyuncs.com/lucas-njfu/bert_flask:v1.3.0
        imagePullPolicy: IfNotPresent # Always
        ports:
        - containerPort: 5000
---
apiVersion: v1
kind: Service
metadata:
  name: bert-flask-service
spec:
  selector:
    app: bert-flask-pod
  type: NodePort # service类型
  ports:
  - port: 5000
    nodePort: 30050 # 指定绑定的node的端口(默认的取值范围是：30000-32767),如果不指定，会默认分配
    targetPort: 5000
```

```kubernetes helm
kubectl create -f bert_flask.yaml -n note
```

检查部署情况
![bert_flask](../image/k8s部署云笔记系统/4.png)

## 5. 部署服务note_cloud

### 5.1 配置ConfigMap

   新建application-prod.yml

```yaml
server:
  port: 8080
  servlet:
    context-path: /api
spring:
  datasource:
    url: jdbc:mysql://note-mysql-service:3306/db_note
    username: root
    password: dfa6f1b032ee42ed95d12647225f2ff4
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource
  redis:
    host: note-redis-service
    port: 6379
    password: adcc64a4a9b04324914626590fb52a1a
    lettuce:
      pool:
        max-active: 10
        max-idle: 10
        min-idle: 1
        time-between-eviction-runs: 10s
    jackson:
      default-property-inclusion: non_null # JSON处理时忽略非空字段
  data:
    mongodb:
      uri: mongodb://note-user:c88c1b8c2ed94010-8372efbc0879bdcb@note-mongodb-service:27017/mongodb_note
  servlet:
    multipart:
      max-file-size: 10MB
      max-request-size: 10MB
jwt:
  base64-encoded-secret-key: lab_of_njust_cloud_computing # jwt的签名密码
bert-base-chinese-service:
  host: bert-flask-service
  port: 5000
```

```kubernetes helm
kubectl create configmap note-cloud-config -n note --fromfile=application-prod.yml
```

   查看部署状态
   ![部署效果](../image/k8s部署云笔记系统/4.png)

### 5.2 部署服务

note-cloud.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: note-cloud-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: note-cloud-pod
  template:
    metadata:
      labels:
        app: note-cloud-pod
    spec:
      containers:
      - name: note-cloud
        image: registry.cn-hangzhou.aliyuncs.com/lucas-njfu/note_cloud:v1.3.0
        imagePullPolicy: IfNotPresent # Always
        ports:
        - containerPort: 8080
        volumeMounts:
        - name: config-volume
          mountPath: /app/config # 配置文件将挂载到容器的/app/config目录
      volumes:
      - name: config-volume
        configMap:
          name: note-cloud-config # 引用之前创建的 ConfigMap
---
apiVersion: v1
kind: Service
metadata:
  name: note-cloud-service
spec:
  selector:
    app: note-cloud-pod
  type: NodePort # service类型
  ports:
  - port: 8080
    nodePort: 30080 # 指定绑定的node的端口(默认的取值范围是：30000-32767),如果不指定，会默认分配
    targetPort: 8080
```

```kubernetes helm
kubectl create -f note-cloud.yaml -n note
```

查看日志

```kubernetes helm
kubectl get pod -n note
kubectl logs -f note-cloud-deployment-6687c5cb49-vc7kl -n note
```

## 6. 部署前端

note-nginx.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: note-nginx-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: note-nginx-pod
  template:
    metadata:
      labels:
        app: note-nginx-pod
    spec:
      containers:
        - name: note-nginx
          image: registry.cn-hangzhou.aliyuncs.com/lucas-njfu/note-nginx:v2.2.0
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: note-nginx-service
spec:
  selector:
    app: note-nginx-pod
  type: NodePort # service类型
  ports:
  - port: 80
    nodePort: 30088 # 指定绑定的node的端口(默认的取值范围是：30000-32767),如果不指定，会默认分配
    targetPort: 80
```

```kubernetes helm
kubectl create -f note-nginx.yaml -n note
```

## 最终部署pod查看

![pod](../image/k8s部署云笔记系统/5.png)

访问(连校园网，关梯子)：

http://192.10.84.209:33395/cloudNote/
账号：姓名小写拼音
密码：123456


最终效果：
![6](../image/k8s部署云笔记系统/6.png)
