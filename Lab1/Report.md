University: ITMO University <br />
Faculty: FICT <br />
Course: Introduction to distributed technologies <br />
Year: 2022/2023 <br />
Group: K4113c <br />
Author:  `Chertkova Anastasia Denisovna ` <br />
Lab: `Lab1 ` <br />
Date of create: 02.12.2022 <br />
Date of finished: .12.2022 <br />


# Лабораторная работа №1 "Установка Docker и Minikube, мой первый манифест."

## Описание
Это первая лабораторная работа в которой вы сможете протестировать Docker, установить Minikube и развернуть свой первый "под".

## Цель работы
Ознакомиться с инструментами Minikube и Docker, развернуть свой первый "под".


>> Данную лабораторную работу рекомендуется начать с изучения Документация по Minikube.


## Перед выполнением лабораторной работы вам необходимо выполнить следующие задачи:

- Установить Docker на рабочий компьютер

- Установить Minikube используя оригинальную инструкцию

# Ход работы

1.  После установки вам необходимо развернуть minikube cluster

` minikube start`

![image](https://user-images.githubusercontent.com/71637557/205462612-b9110e58-01e7-4285-a1e0-47ff052296de.png)

2.  После запуска minikube cluster вы сможете взаимодействовать с k8s используя команду:

` minikube kubectl `

![image](https://user-images.githubusercontent.com/71637557/205463549-e25f7054-72c8-4ef3-b202-de9189763db8.png)


3. Для первого манифеста мы выбрали образ HashiCorp Vault. Вам нужно будет написать манифест для развертывания "пода" HashiCorp Vault, и при этом прокинуть внутрь порт 8200

-- Скачиваем образ vault
`docker pull vault `
![image](https://user-images.githubusercontent.com/71637557/205464893-18182656-9854-4ca6-bb75-661ea47ae31d.png)

-- Создаем контейнер на основе образа vault 
`docker run -d --name vault vault`
![image](https://user-images.githubusercontent.com/71637557/205464852-8b8d99be-777c-4fab-a744-f48d430b83e1.png)

-- Создаем манифест для развертывания "пода" HashiCorp Vault, и при этом прокинуть внутрь порт 8200

```
apiVersion: v1
kind: Pod
metadata:
  name: vault
  labels:
    environment: dev
    tier: vault
spec:
  containers:
    - name: vault
      image: vault
      ports:
        - containerPort: 8200

```
Создаем под и проверям прошла ли операция.

![image](https://user-images.githubusercontent.com/71637557/205465423-c2f20f64-0c8b-4bb6-a4c7-552261138609.png)


4. После этого вам необходимо будет создать сервис для доступа к этому контейнеру, самый просто вариант - это выполнить команду:

> Эта команда будет работать только если ваш "под" имеет имя vault

` minikube kubectl -- expose pod vault --type=NodePort --port=8200 `

5. Команда выше создаст сервис, но как же вам попасть на ваш контейнер? Воспользуйтесь следующей командой:

` minikube kubectl -- port-forward service/vault 8200:8200 `

6. minikube прокинет порт вашего компьютера в контейнер и вы сможете зайти в vault по ссылке http://localhost:8200

7. После перехода по ссылке у вас откроется интерфейс как на рисунке ниже

![image](https://user-images.githubusercontent.com/71637557/205458681-ac16c98c-8576-4137-b43f-c2ec025eecbc.png)

8. Для успешного завершения лабораторной работы вам необходимо войти в ваш vault ипользуя токен, который вам необходимо НАЙТИ, а не сгенерировать.

9. Теперь вопросы на засыпку: 1. Что сейчас произошло и что сделали команды указанные ранее? 2. Где взять токен для входа в Vault? (Подсказка: Логи всему голова)

10. Для остановки minikube cluster вы можете воспользоваться командой

` minikube stop `

_____________________________

# DOCKER

### Создаем Docker контейнер

Docker - это одна из систем, позволяющих контейнеризировать приложения. 

#### Контейнер (container) и образ (image):

Контейнер - подобен запущенной мини-виртуальной машине с изолированным внутри приложением.<br />

Образ - это шаблон для запуска такой "машины".

### Приступим к созданию контейнера:

Шаг 1.

В Windows:

Cоздаем папку lab1, поместите в нее программу на Питон с именем 1.py и файл с названием Dockerfile без расширения

* Dockerfile

Файл Dockerfile содержит набор инструкций, следуя которым Docker будет собирать образ контейнера. Этот файл содержит описание базового образа, который будет представлять собой исходный слой образа. Среди популярных официальных базовых образов можно отметить python, ubuntu, alpine.

```
FROM python:3.6-alpine3.8                     # Скачиваем легковесный образ python-alpine <br />
COPY 1.py /app/                               # Копируем 1.py с компьютера в дирректорию /app образа <br />
WORKDIR /app                                  # Делаем /app рабочей дирректорией для след. команды <br />
ENTRYPOINT ["python3", "/app/1.py"]           # Говорим "Питон3, запусти приложение /app/1.py" <br />

```

* 1.py

` print("Hello world") `

Шаг 2. 

Собираем наш Докер-образ. В терминале, в директории с Докерфайлом и приложением запускаем команду:

` docker build . -t py_app:v0.1 `

Таким образом из Докерфайла в текущей директории (точка) соберётся образ с тэгом py_app:v0.1

Увидеть список образов можно командой 

` docker images `

Чтобы запустить контейнер, напишите

`  docker run py_app:v0.1 `

____________________

# MINIKUBE

minikube is local Kubernetes, focusing on making it easy to learn and develop for Kubernetes.

Minikube - утилита командной строки для настройки и запуска однонодового кластера Kubernetes в виртуальной машине на локальном компьютере.

__________________________

# Kubernetes (K8S)

Kubernetes, также известная как K8s, представляет собой систему с открытым исходным кодом для автоматизации развертывания, масштабирования и управления контейнерными приложениями.

Pod в Kubernetes - это минимальная единица, в составе Пода может быть несколько контейнеров. У каждого пода есть уникальный IP-адрес. Для описания пода пишут манифест (manifest file).

## Архитектура Kubernetes

Работающий кластер Kubernetes включает в себя агента, запущенного на нодах (kubelet) и компоненты мастера (APIs, scheduler, etc), поверх решения с распределённым хранилищем.

![image](https://user-images.githubusercontent.com/71637557/205464202-c7b3f8eb-d5c1-40b3-af93-8853cb394d73.png)

### Нода Kubernetes

При взгляде на архитектуру системы мы можем разбить его на сервисы, которые работают на каждой ноде и сервисы уровня управления кластера. На каждой ноде Kubernetes запускаются сервисы, необходимые для управления нодой со стороны мастера и для запуска приложений. Конечно, на каждой ноде запускается Docker. Docker обеспечивает загрузку образов и запуск контейнеров.

### Kubelet

Kubelet управляет pod'ами их контейнерами, образами, разделами, etc.

### Kube-Proxy

Также на каждой ноде запускается простой proxy-балансировщик. Этот сервис запускается на каждой ноде и настраивается в Kubernetes API. Kube-Proxy может выполнять простейшее перенаправление потоков TCP и UDP (round robin) между набором бэкендов.


# Ссылки для работы

https://www.docker.com
https://minikube.sigs.k8s.io/docs/
https://minikube.sigs.k8s.io/docs/start/
https://docs.docker.com/get-started/
https://kubernetes.io/ru/docs/tutorials/hello-minikube/
https://kubernetes.io/ru/docs/tutorials/kubernetes-basics/
https://kubernetes.io/docs/home/
https://habr.com/ru/post/258443/
https://stepik.org/lesson/550144/step/2?unit=543781
https://hub.docker.com/_/vault/
https://www.vaultproject.io

