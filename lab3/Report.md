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


2. Вам необходимо создать replicaSet с 2 репликами контейнера ifilyaninitmo/itdt-contained-frontend:master и используя ранее созданный configMap передать переменные REACT_APP_USERNAME, REACT_APP_COMPANY_NAME .

Пример манифеста ReplicaSet

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: replicas
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

## 3. Включить minikube addons enable ingress и сгенерировать TLS сертификат, импортировать сертификат в minikube.

Созданим для начало сервис. 

```
apiVersion: v1
kind: Service
metadata:
  name: sv
spec:
  type: NodePort
  selector:
    app: lab3
  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000
```

![image](https://user-images.githubusercontent.com/71637557/208991799-87639770-683a-4987-be2e-3669e18f8644.png)

Генерируем сертификат TLS с использованием OpenSSL.

Запускаем opensll и генерируем приватный ключ RSA. Создаем подпись сертификата (CSR).

![image](https://user-images.githubusercontent.com/71637557/208966612-7e3ccf14-4281-4e43-b4e7-0b7e5adadadd.png)

Подписываем сертификат тем же ключом, с помощью которого он был создан.
![image](https://user-images.githubusercontent.com/71637557/208967521-1de097fe-f3de-484b-b9d9-1f28cf794120.png)

Создаем секрет

![image](https://user-images.githubusercontent.com/71637557/208971811-f95f6c2c-7084-48fc-a208-88b83d96d1c7.png)

Создаем Ingress. Подключаем Ingress в minikube.

![image](https://user-images.githubusercontent.com/71637557/208973821-31b64795-6357-47c2-940b-fbd83ba79584.png)


## 4. Создать ingress в minikube, где указан ранее импортированный сертификат, FQDN по которому вы будете заходить и имя сервиса который вы создали ранее.

Созданим манифест для Ingress

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress
spec:
  tls:
  - hosts:
      - anastasia
    secretName: sec
  rules:
  - host: anastasia
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: serv
            port:
              number: 3000
```

![image](https://user-images.githubusercontent.com/71637557/208976560-0fcd765e-9f5d-4654-b6b3-37ed4a5b6b65.png)

## 5. В hosts пропишите FQDN и IP адрес вашего ingress и попробуйте перейти в браузере по FQDN имени.

![image](https://user-images.githubusercontent.com/71637557/208975791-3ed823a1-7ae5-4702-bde6-ac41aff5282d.png)


## 6. Войдите в веб приложение по вашему FQDN используя HTTPS и проверьте наличие сертификата.



# Теория

ConfigMaps - Это основной ресурс для хранения в Кубенетес конфигурационных данных. Его отличие от секрета в том, что в Секретах данные кодируются в base64. Также удобно иметь разные типы ресурсов для разных целей, ведь к ним можно будет предоставить разные доступа. <br />

ReplicaSet - ресурс K8S, который гарантирует работу определенного числа реплик Pod.<br /> 

Ingress - Объект API, который управляет внешним доступом к службам в кластере, обычно HTTP. <br />
Ingress может обеспечить балансировку нагрузки, завершение работы SSL и виртуальный хостинг на основе имен.<br />

Secrets -  это объект, содержащий небольшое количество конфиденциальных данных, таких как пароль, токен или ключ.<br />

TLS сертификат - SSL расшифровывается как Secure Sockets Layer, TLS – Transport Layer Security. Сертификат представляет собой технологию безопасности, с помощью которой шифруется связь между браузером и сервером. <br />

Сертификат TLS — это цифровой сертификат, соответствующий стандарту X.509, который предъявляется в процессе использования криптографического протокола TLS. В свою очередь, X.509 — это общий стандарт, описывающий структуру сертификата. <br />

# Полезные материалы

https://kubernetes.io/docs/concepts/configuration/configmap/ <br />
https://stepik.org/lesson/593295/step/1?unit=588304 <br />
https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/   <br />
https://kubernetes.io/docs/concepts/services-networking/service/  <br />
https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/#enable-the-ingress-controller  <br />
https://itsecforu.ru/2019/11/26/🗝%EF%B8%8F-как-создать-ca-и-генерировать-серти/  <br />
https://stepik.org/lesson/550147/step/1?unit=543784  <br />
https://habr.com/ru/company/globalsign/blog/689984/ <br />
https://kubernetes.io/docs/concepts/configuration/secret/ <br />
https://falcongaze.com/ru/pressroom/publications/research/ssl-tls.html

