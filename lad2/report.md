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
