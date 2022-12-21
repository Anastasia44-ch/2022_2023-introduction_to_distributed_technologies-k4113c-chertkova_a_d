University: ITMO University <br />
Faculty: FICT <br />
Course: Introduction to distributed technologies <br />
Year: 2022/2023 <br />
Group: K4113c <br />
Author:  `Chertkova Anastasia Denisovna ` <br />
Lab: `Lab3 ` <br />
Date of create: 09.12.2022 <br />
Date of finished: .12.2022 <br />



# Лабораторная работа №3 "Сертификаты и "секреты" в Minikube, безопасное хранение данных."

# Описание

В данной лабораторной работе вы познакомитесь с сертификатами и "секретами" в Minikube, правилами безопасного хранения данных в Minikube.

# Цель работы

Познакомиться с сертификатами и "секретами" в Minikube, правилами безопасного хранения данных в Minikube.

# Ход работы

1. Вам необходимо создать configMap с переменными: REACT_APP_USERNAME, REACT_APP_COMPANY_NAME.

Пример манифеста configMap

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: configmap
data:
  react_app_user_name: "Anastasia"
  react_app_company_name: "ITMO"
```
Сохранение этого манифеста в config.yaml и отправим его в кластер Kubernetes. 

![image](https://user-images.githubusercontent.com/71637557/208948338-0ed541bf-340f-46c8-8987-55f16e93b483.png)

2. Вам необходимо создать replicaSet с 2 репликами контейнера ifilyaninitmo/itdt-contained-frontend:master и используя ранее созданный configMap передать переменные REACT_APP_USERNAME, REACT_APP_COMPANY_NAME .

Пример манифеста ReplicaSet

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: replicaset
  labels:
    app: lab3
spec:
  replicas: 2
  selector:
    matchLabels:
      app: lab3
  template:
    metadata:
      labels:
        app: lab3
    spec:
      containers:
      - name: container
        image: ifilyaninitmo/itdt-contained-frontend:master
        ports:
        - containerPort: 3000
        env:
        - name: REACT_APP_USERNAME
          valueFrom:
            configMapKeyRef:
              name: configmap
              key: react_app_user_name
        - name: REACT_APP_COMPANY_NAME
          valueFrom:
            configMapKeyRef:
              name: configmap
              key: react_app_company_name
```

Сохранение этого контролера в config.yaml и отправим его в кластер Kubernetes. 

![image](https://user-images.githubusercontent.com/71637557/208952865-be593f5e-c747-433c-980a-efe27b9a1a5b.png)

3. Включить minikube addons enable ingress и сгенерировать TLS сертификат, импортировать сертификат в minikube.

Созданим для начало сервис. 

```
apiVersion: v1
kind: Service
metadata:
  name: serv
spec:
  type: NodePort
  selector:
    app: lab3
  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000
```

![image](https://user-images.githubusercontent.com/71637557/208955988-3561fdd8-6562-4df4-9637-f5835ae113ad.png)



# Теория

# Полезные материалы

https://kubernetes.io/docs/concepts/configuration/configmap/ <br />
https://stepik.org/lesson/593295/step/1?unit=588304 <br />
https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/   <br />
https://kubernetes.io/docs/concepts/services-networking/service/  <br />
https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/#enable-the-ingress-controller  <br />
https://stepik.org/lesson/550147/step/1?unit=543784  <br />
https://kubernetes.io/docs/concepts/configuration/secret/

