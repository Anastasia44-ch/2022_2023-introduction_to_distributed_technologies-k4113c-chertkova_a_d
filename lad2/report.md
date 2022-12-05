University: ITMO University <br />
Faculty: FICT <br />
Course: Introduction to distributed technologies <br />
Year: 2022/2023 <br />
Group: K4113c <br />
Author:  `Chertkova Anastasia Denisovna ` <br />
Lab: `Lab2 ` <br />
Date of create: 05.12.2022 <br />
Date of finished: .12.2022 <br />



# Лабораторная работа №2 "Развертывание веб сервиса в Minikube, доступ к веб интерфейсу сервиса. Мониторинг сервиса."

## Описание

В данной лабораторной работе вы познакомитесь с развертыванием полноценного веб сервиса с несколькими репликами.

## Цель работы

Ознакомиться с типами "контроллеров" развертывания контейнеров, ознакомится с сетевыми сервисами и развернуть свое веб приложение.

# Ход работы

1. Вам необходимо создать deployment с 2 репликами контейнера ifilyaninitmo/itdt-contained-frontend:master и передать переменные в эти реплики: REACT_APP_USERNAME, REACT_APP_COMPANY_NAME.

  1.1 Скачиваем образ  ifilyaninitmo/itdt-contained-frontend:master
  
  > команда docker pull ifilyaninitmo/itdt-contained-frontend:master

![image](https://user-images.githubusercontent.com/71637557/205738378-ea9602d4-69d4-4b83-ae17-3976603328d3.png)
  
  1.2 Создаем контейнер на основе образа
  
  > команда docker run -d --name container ifilyaninitmo/itdt-contained-frontend:master

![image](https://user-images.githubusercontent.com/71637557/205738994-4656de28-7247-4f57-9b13-52bddd8fb191.png)

  1.3 Сохраним данный манифест в файл Deployment.yaml и запустим его в кластере Kubernetes с помощью команды:
  
  > kubectl create -f "C:\progect\lab2\Deployment.yaml"
  
  ``
  apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend-deployment
  labels:
    app: frontend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers: 
      - name: frontend-container
        image: ifilyaninitmo/itdt-contained-frontend:master
        ports:
        - containerPort: 3000
        env:
          - name: REACT_APP_USERNAME
            value: Anastasia
          - name: REACT_APP_COMPANY_NAME
            value: ITMO
  
  ``

![image](https://user-images.githubusercontent.com/71637557/205740671-99c35729-8735-4d2e-aa7b-9f647d348d7e.png)

______
* развертывание создает 2 экземпляра пода (количество указано в поле replicas);
* в поле селектора указано, как развертывание (Deployment) обнаружит, какими подами (Pods) нужно управлять. В этом манифесте просто выбираем одну метку, определенную в шаблоне Pod‘а (app: frontend);
* описание шаблона Pod‘а в поле template: spec “требует” запустить docker-контейнер frontend, из образа frontend. Данному поду будет присвоена метка app: frontend;
* развертывание открывает 3000-й порт контейнера.
* Шаблон пода задается в объекте Template. С помощью свойства env объявляем внутри подов переменные окружения REACT_APP_USERNAME и REACT_APP_COMPANY_NAME со значениями Anastasia и ITMO, соответственно.
______

2. Создать сервис через который у вас будет доступ на эти "поды". Выбор типа сервиса остается на ваше усмотрение.



3. Запустить в minikube режим проброса портов и подключитесь к вашим контейнерам через веб браузер.

4. Проверьте на странице в веб браузере переменные REACT_APP_USERNAME, REACT_APP_COMPANY_NAME и Container name. Изменяются ли они? Если да то почему?

5. Проверьте логи контейнеров, приложите логи в отчёт.




## Немного теории

У контроллера Deployment - Вы описываете yaml-манифест того, в каком состоянии Вы хотите видеть свое приложение, а Деплоймент сам об этом позаботится. Деплоймент - самый популярный контроллер в Кубернетес, почти никогда Поды не запускают без Деплоймента. Подобно Сервису Деплоймент находит Поды себе под контроль с помощью Лейблов.

Deployment имеет возможность делать плавающие/постепенные обновления. То есть, сначала ставить Под с новой версией приложения, и когда она поднимется, убивать реплику старой версии приложения. Такая политика позволит сделать обновление более плавным и безопасным. 

Как и для всех других API-объектов Kubernetes, для определения Deployment в yaml-файле нужны поля apiVersion, kind и metadata. Кроме того, в Deployment также должна присутствовать секция .spec.

## Полезные ссылки для работы

https://kubernetes.io/docs/concepts/workloads/controllers/deployment/  <br />
https://docs.docker.com/engine/reference/commandline/pull/ <br />
https://itnan.ru/post.php?c=1&p=448094 <br />
https://gb.ru/posts/kak-razvernut-prilozhenie-na-kubernetes <br />
https://ealebed.github.io/posts/2018/знакомство-с-kubernetes-часть-5-deployments/ <br />
https://stepik.org/lesson/550146/step/1?unit=543783 <br />
https://itisgood.ru/2020/02/27/shpargalka-po-kubernetes-service/
