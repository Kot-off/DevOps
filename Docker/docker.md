# Docker

### Установка Docker одной командой https://docs.docker.com/desktop/install/ubuntu/

```
curl -sSL https://get.docker.com/ | sh
```

### Установка Compose plugin https://docs.docker.com/compose/install/linux/

```
sudo apt-get install -y docker-compose
```

### Создание user docker https://docs.docker.com/engine/install/linux-postinstall/

```
sudo groupadd docker
useradd -m -s /bin/bash -g docker docker
sudo usermod -aG docker docker
newgrp docker
```
