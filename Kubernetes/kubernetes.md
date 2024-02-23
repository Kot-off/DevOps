# Kubernetes

- Namespace (Неймспейс)
- Pod (Под)
- Deployment (Деплоймент)
- Service (Сервис)
- Ingress (Ингрес)
- Configmap (Конфигмап)
- Secret (Секрет)

#####################

## Namespace

#####################

## Namespace (Неймспейс) в Kubernetes - объект, изолирующий ресурсы и большую часть объктов в кластере

(любые объекты в кластере должны работать в Namespace).

### При создании кластера Kubernetes по умолчанию пользователю доступно 4 неймспейса:

- default
- kube-node-lease
- kube-public //объеты которые должны быть доступны всем, по задумке разработчика
- kube-system //системные компоненты отвечающие за работу кластера

#####################

## Pod

#####################

## Pod (Под) - Минимальная вычислительная единица в Kubernetes для запуска приложений.

Под состоит из одного или нескольких контейнеров с общим сетевым пространством и возможностью
иметь общее хранилище.

Пример простого манифеста пода в Kubernetes:

```
apiVersion: v1
kind: Pod
metadate:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
```

#####################

## Deployment

#####################

## Deployment (Деплоймент) - объект, реализующий обновлнеие новой версии Подов и позволяющий

заупустить приложение в нескольких репликах.
Deployment будет отслеживать, то кол-во Pod (Подов) которое мы указали. И задача Deployment-a
в том, чтобы эти реплики переподнять.

### Пример простого Deployment (Деплоймента) в Kubernetes:

```
apiVersion: apps/v1
kind: Deployment
metadate:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    metchLabels:
	  app: nginx
  template:
    metadata:
	labels:
    app: nginx
  spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
```

#####################

## Service

#####################

## Service (Сервис) - объект, предоставляющий сетевой доступ через доменное имя к одному Поду

или балансировку нескольким Подам с одинаковыми лейблами.
Service - это постоянная гарантия взаимодействия через доменное имя.

- Представленный ниже сервис позволяет взаимодействовать с подом nginx через доменное имя nginx.
  Такая связанность достигается путем одинаковыз значений `app: nginx` в selector у Service и labels у пода.

### Пример манифеста:

```
apiVersion: v1
kind: Service
metadate:
  name: nginx
spec:
  type: ClusterIP
  selector:
	app: nginx
  ports:
    - protocol: TCP
	  port: 80
      targetPort: 80
---
apiVersion: v1
kind: Pod
metadate:
  name: nginx
  labels:
    app: nginx
  spec:
    containers:
    - name: nginx
      image: nginx:1.14.2
```

### В Kubernetes у объекта Service существует 5 видов работы

- type: ClusterIP - Делает приложение доступным по доменному имени внутри кластера
- type: NodePort - Делает приложение доступным изнутри кластера (во внешней сети) через ip адрес воркер нод и
  nodeport в диапазоне 30000-32767
- type: LoadBalancer - Делает приложение доступным изнутри кластера через внешний ip адрес. По умолчанию не
  работает в кластере. Работает в облачных кластерах или установленных реализаций например metalLb
- type: ExternalName - Позволяет реализовать CNAME на любом DNS имя за пределами кластера, например сервис с
  названием my-service.prod.svc.cluster.local будет проксировать запрос на my.database.example.com
- ClusterIP: None - Делает приложение доступным внутри кластера по имени пода. Не осуществляет балансировку,
  поэтому вам необходимо знать конечную точку адресата.

####################

## Ingress

####################

## Ingress (Ингрес) - объект, управляющий внешним доступом до наших сервисов в кластере через HTTP или HTTPS

которые так же осуществляют балансировку, SSL терминацию и возможность указание хостов в виде доменов.
Для его реализации и работоспособности требуется установленный Ingress Controller.

### Команда для установки ингрес контроллера

```
kubectl create ns ingress-nginx
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
helm -n ingress-nginx install ingress-nginx oci://registry-1.docker.io/bitnamicharts/nginx-ingress-controller
```

### Пример манифеста:

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadate:
  name: name-virtual-host-ingress
spec:
  rules:
  - host: nginx.test.com
    http:
      paths:
      - pathType: Prefix
		path: "/"
	    backend:
          service:
			name: nginx
			port:
			  number: 80
```

####################

## Команды:

####################

### Посмотреть список неймспейсов

```
kubectl get ns
```

### Создать неймспейс (в данном примере создали неймспейс test)

```
kubectl create ns test
```

### Запустить под с образом nginx в неймспейсе test

```
kubectl run nginx --image=nginx --namespace=test
```

### Получить список подов в неймспейсе test

```
kubectl get pods --namespace=production
```

### Создание пода flight-info-service в неймспейсе back

```
kubectl apply -n back -f flight-info-service-deployment.yaml
```

### Список Deployment (Деплойментов)

```
kubectl get deploy
```

- в таком виде команды выведет неймспейсы default деплоумента
- необходимо указывать `-n front`, в данном примере выведем деплоументы неймспейса front

### Команда для создания пода из файла/интернета

```
kubectl apply -f service-pod.yml
```

### Команда показывает сервис - описанный в пункте Service

```
kubectl get svc
```

### Команда для установки ингрес контроллера

```
kubectl create ns ingress-nginx
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
helm -n ingress-nginx install ingress-nginx oci://registry-1.docker.io/bitnamicharts/nginx-ingress-controller
```

### Команда для проверки

```
curl -H "Host: nginx.test.com <наш хост>" <внешний ip> -I
```
