# Docker

## Установка и настройка

### [Установка](https://docs.docker.com/desktop/install/ubuntu/) Docker одной командой

```
curl -sSL https://get.docker.com/ | sh
```

### Установка [Compose plugin](https://docs.docker.com/compose/install/linux/)

```
sudo apt-get install -y docker-compose
```

### Создание [user docker](https://docs.docker.com/engine/install/linux-postinstall/)

```
sudo groupadd docker
useradd -m -s /bin/bash -g docker docker
sudo usermod -aG docker docker
newgrp docker
```

---

## Основные команды

### Статус докера

```
service docker status
```

### Посмотреть Images

```
docker images
```

### Посмотреть запущенные контейнеры Docker

```
docker ps
```

- флаг `-a` покажет остановленные контейнеры

### Docker [inspect](https://docs.docker.com/engine/reference/commandline/inspect/) предоставляет подробную информацию о конструкциях, управляемых Docker

```
docker inspect <id image>
```

### Удалить контейнер docker

```
docker rm <id image>
```

- флаг `-f` позволяет удалить запущенные контейнеры

### Удалить images docker

```
docker rmi <id image>
```

### Создать Docker контейнер можно командой

```
docker run -d nginx
```

- флаг `-d` запускает в фоновом режиме
- флаг `--rm` удалит контейнер при остановке

### Так же контейнер можно остановить, запустить, перезапустить

```
docker start -d nginx
```

- start
- stop
- restart

### Посмотреть сколько ресурсов требует контейнер

```
docker stats <id>
```

### Зайти в контейнер

```
docker exec -it <id> /bin/bash
```

### Удалить все контейнеры, images, volume

```
docker system prune -a --volumes
```

### Посмотреть volume

```
docker volume ls
```

### Посмотреть [network](https://docs.docker.com/engine/reference/commandline/network_ls/) Docker

```
docker network ls
```

### Создаём сеть Docker

```
docker network create --driver <Указывается тип сети который нам нужен> <Ваше имя сети>
```

- Docker позволяет использовать [5 режимов](https://rohanzi.gitlab.io/balberin-clouds/project/networks/) сети:
  - bridge - программный режим моста. Docker по умолчанию использует сеть с названием bridge для всех контейнеров для общения в пределах одной машины, если для них не описываются другие сети.
  - overlay - распределенная сеть между множественными хостами Docker для использования сервисами. В Swarm по умолчанию используется сеть типа overlay с именем ingress для распределения нагрузки, а так же сеть типа bridge с названием docker_gwbridge для коммуникации самих Docker daemon.
  - host - изолирует контейнер до той степени, что к нему можно обращаться только в пределах Docker-хоста. Может использоваться с режимом swarm, при этом overlay драйвер так же будет активен, но это вносит ряд ограничений и вряд ли имеет смысл.
  - none - отключает все сети. Часто используется в дополнении при описании нестандартных сетей, чтобы отключить все лишнее. Режим не может использоваться с Swarm.
  - macvlan - позволяет привязать контейнеру MAC-адрес, тем самым позволяет подключиться к физической сети. Рекомендуется для использования с теми приложениями, что требуют прямого доступа к сети. Не может использоваться при описании файла конфигурации Docker Compose. Поддерживает режимы bridge и 802.1q. Можно создать ipvlan, если нужен L2 мост вместо L3.

### Посмотреть информацию о созданной сети

```
docker network inspect <имя сети>
```

### Создание network с указанием ip

```
docker network create -d <Указывается тип сети который нам нужен> --subnet 192.168.10.0/24 --geteway 192.168.10.1 <Ваше имя сети>
```

### С помощью команды можно подключить один контейнер в сеть к другому

```
docker network connect <имя сети> <имя контейнера>
```

### Отключить контейнер от старой сети

```
docker network disconnect <network id можно посмотреть через inspect> <имя контейнера>
```

###

```
docker network inspect <имя сети>
```

###

```
docker network inspect <имя сети>
```
