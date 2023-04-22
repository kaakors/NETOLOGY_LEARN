# Домашнее задание к занятию «Сетевое взаимодействие в K8S. Часть 1»

### Цель задания

В тестовой среде Kubernetes необходимо обеспечить доступ к приложению, установленному в предыдущем ДЗ и состоящему из двух контейнеров, по разным портам в разные контейнеры как внутри кластера, так и снаружи.

------

### Чеклист готовности к домашнему заданию

1. Установленное k8s-решение (например, MicroK8S).
2. Установленный локальный kubectl.
3. Редактор YAML-файлов с подключённым Git-репозиторием.

------

### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания

1. [Описание](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) Deployment и примеры манифестов.
2. [Описание](https://kubernetes.io/docs/concepts/services-networking/service/) Описание Service.
3. [Описание](https://github.com/wbitt/Network-MultiTool) Multitool.

------

### Задание 1. Создать Deployment и обеспечить доступ к контейнерам приложения по разным портам из другого Pod внутри кластера

1. Создать Deployment приложения, состоящего из двух контейнеров (nginx и multitool), с количеством реплик 3 шт.
2. Создать Service, который обеспечит доступ внутри кластера до контейнеров приложения из п.1 по порту 9001 — nginx 80, по 9002 — multitool 8080.
3. Создать отдельный Pod с приложением multitool и убедиться с помощью `curl`, что из пода есть доступ до приложения из п.1 по разным портам в разные контейнеры.
4. Продемонстрировать доступ с помощью `curl` по доменному имени сервиса.
5. Предоставить манифесты Deployment и Service в решении, а также скриншоты или вывод команды п.4.

		apiVersion: v1
		kind: Service
		metadata:
  		name: my-service
		spec:
			selector:
				app: nginx
			ports:
				- protocol: TCP
					port: 9001
					targetPort: 80
					name: nginx
				- protocol: TCP
					port: 9002
					targetPort: 8080
					name: http-multitool

		apiVersion: apps/v1
		kind: Deployment
		metadata:
			name: nginx-deployment
			labels:
				app: nginx
		spec:
			replicas: 3
			selector:
				matchLabels:
					app: nginx
			template:
				metadata:
					labels:
						app: nginx
				spec:
					containers:
					- name: nginx
						image: nginx:1.14.2
						ports:
						- containerPort: 80
							name: http-nginx
					- name: network-multitool
						image: wbitt/network-multitool
						env:
						- name: HTTP_PORT
							value: "8080"
						- name: HTTPS_PORT
							value: "11443"
						ports:
						- containerPort: 8080
							name: http-multitool
          
  
<img width="1410" alt="Снимок экрана 2023-04-22 в 23 14 11" src="https://user-images.githubusercontent.com/99620296/233804540-31ab84cf-d93e-42b9-8577-3732e5c089b0.png">



------

### Задание 2. Создать Service и обеспечить доступ к приложениям снаружи кластера

1. Создать отдельный Service приложения из Задания 1 с возможностью доступа снаружи кластера к nginx, используя тип NodePort.
2. Продемонстрировать доступ с помощью браузера или `curl` с локального компьютера.
3. Предоставить манифест и Service в решении, а также скриншоты или вывод команды п.2.

		apiVersion: v1
		kind: Service
		metadata:
			name: my-service
		spec:
			type: NodePort
			selector:
				app: nginx
			ports:
				- protocol: TCP
					port: 9001
					targetPort: 80
					name: nginx
				- protocol: TCP
					port: 9002
					targetPort: 8080
					name: http-multitool
					
		<!DOCTYPE html>
		<html>
		<head>
		<title>Welcome to nginx!</title>
		<style>
				body {
						width: 35em;
						margin: 0 auto;
						font-family: Tahoma, Verdana, Arial, sans-serif;
				}
		</style>
		</head>
		<body>
		<h1>Welcome to nginx!</h1>
		<p>If you see this page, the nginx web server is successfully installed and
		working. Further configuration is required.</p>

		<p>For online documentation and support please refer to
		<a href="http://nginx.org/">nginx.org</a>.<br/>
		Commercial support is available at
		<a href="http://nginx.com/">nginx.com</a>.</p>

		<p><em>Thank you for using nginx.</em></p>
		</body>
		</html>



------

### Правила приёма работы

1. Домашняя работа оформляется в своем Git-репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl` и скриншоты результатов.
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.
