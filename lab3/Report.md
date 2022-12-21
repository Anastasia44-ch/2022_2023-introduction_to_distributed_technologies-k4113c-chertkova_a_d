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

Пример манифеста

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: configmap
data:
  react_app_user_name: "Anastasia"
  react_app_company_name: "ITMO"

```



# Теория

# Полезные материалы

https://kubernetes.io/docs/concepts/configuration/configmap/ <br />
https://stepik.org/lesson/593295/step/1?unit=588304 <br />
https://stepik.org/lesson/550147/step/1?unit=543784  <br />
