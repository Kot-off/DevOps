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

### Docker inspect предоставляет подробную информацию о конструкциях, управляемых Docker https://docs.docker.com/engine/reference/commandline/inspect/

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

### Так же контейнер можно остановить, запустить, перезапустить

```
docker start -d nginx
```

- start
- stop
- restart

### ...

```
docker start -d nginx
```
