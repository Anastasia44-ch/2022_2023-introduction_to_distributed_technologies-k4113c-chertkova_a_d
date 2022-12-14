University: ITMO University <br />
Faculty: FICT <br />
Course: Introduction to distributed technologies <br />
Year: 2022/2023 <br />
Group: K4113c <br />
Author:  `Chertkova Anastasia Denisovna ` <br />
Lab: `Lab4 ` <br />
Date of create: 20.12.2022 <br />
Date of finished: .12.2022 <br />



# Лабораторная работа №4 "Сети связи в Minikube, CNI и CoreDNS"

## Описание

Это последняя лабораторная работа в которой вы познакомитесь с сетями связи в Minikube. Особенность Kubernetes заключается в том, что у него одновременно работают underlay и overlay сети, а управление может быть организованно различными CNI.

## Цель работы

Познакомиться с CNI Calico и функцией IPAM Plugin, изучить особенности работы CNI и CoreDNS.

## Ход работы

1. При запуске minikube установите плагин CNI=calico и режим работы Multi-Node Clusters одновеременно, в рамках данной лабораторной работы вам нужно развернуть 2 ноды.

``` 
minikube start --network-plugin=cni --cni=calico --nodes 2 --kubernetes-version=v1.25.3
```
![image](https://user-images.githubusercontent.com/71637557/209014629-ed6733e2-cbfc-4d63-bc02-17c564730ed3.png)

2. Посмотрим список созданных узлов

![image](https://user-images.githubusercontent.com/71637557/209067550-a331dbac-e240-4f2a-a20d-fd85a6bbc189.png)

3. Развернем модуль calicoctl

``` 
kubectl apply -f calicoctl.yaml
```
![image](https://user-images.githubusercontent.com/71637557/209020289-7087bd77-e068-4252-8220-2d5e4733a941.png)

4. Создадим ip pool, удалим ippool по умолчанию:

``` 
kubectl exec -i -n kube-system calicoctl -- /calicoctl create -f - < ip_pool.yaml
kubectl exec -i -n kube-system calicoctl -- /calicoctl  delete ippools default-ipv4-ippool
kubectl exec -i -n kube-system calicoctl -- /calicoctl  get ippools -o wide
``` 
5. Давайте создадим ресурсы, сервис, карту конфигурации.
```
kubectl apply -f resources.yaml
```
![image](https://user-images.githubusercontent.com/71637557/209917776-6b36735a-acd8-4976-94d5-a8277f14c9ab.png)

6. Проверьте IP-адреса наших pods.
```
kubectl get pods -o wide
```
![image](https://user-images.githubusercontent.com/71637557/209918859-c26767b7-d7da-4a31-ba59-f16f36bd40e1.png)

![image](https://user-images.githubusercontent.com/71637557/209915331-0d0bb6d7-5635-469e-aefd-a7d0b9fb9aad.png)


7.Попингуем поды
```
kubectl exec -it lab4-d4h9x  -- ping -c4 192.168.2.1
```
![image](https://user-images.githubusercontent.com/71637557/209921207-d2c66a9f-17c6-4351-9ad1-643bd032fea1.png)


![Снимок экрана (3)](https://user-images.githubusercontent.com/71637557/209048709-aab417be-8bd8-4435-8499-5e5d6e9dfd54.png)


# Теория

# Полезные ссылки
https://projectcalico.docs.tigera.io/getting-started/kubernetes/minikube <br />
https://minikube.sigs.k8s.io/docs/tutorials/multi_node/  <br />
https://habr.com/ru/company/flant/blog/485716/


