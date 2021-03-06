# Технологии разработки программного обеспечения

## Лабораторная работа № 2: создание кластера Kubernetes и деплой приложения

-  Выполнила: Ледяева Г.А
-  Группа: МБД2131

-  Цель лабораторной работы: Знакомство с кластерной архитектурой  Kubernetes, а также деплой приложения в кластер.

### Манифесты 

- deployment.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sampleapi-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: sample-app
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: sample-app
    spec:
      containers:
        - image: sampleapi:latest
          imagePullPolicy: Never 
          name: sample
          ports:
            - containerPort: 8080
      hostAliases:
      - ip: "192.168.49.1" # The IP of localhost from MiniKube
        hostnames:
        - postgres.local
  ```
        
- service.yaml 

```
apiVersion: v1
kind: Service
metadata:
  name: sampleapi-service
spec:
  type: NodePort
  ports:
    - nodePort: 31317
      port: 8080
      protocol: TCP
      targetPort: 8080
  selector:
    app: sampleapi-app
 ```
### Список запущенных подов

![Image alt](https://github.com/Galina-lina/SampleApi_Kuber/blob/main/pods.png)

### Логи пода

![Image alt](https://github.com/Galina-lina/SampleApi_Kuber/blob/main/podlog.png)

![Image alt](https://github.com/Galina-lina/SampleApi_Kuber/blob/main/podlog2.png)

### Осмотр подов в Dashboard Kebernetes

![Image alt](https://github.com/Galina-lina/SampleApi_Kuber/blob/main/graph_interface1.png)

![Image alt](https://github.com/Galina-lina/SampleApi_Kuber/blob/main/graph_interface2.png)
