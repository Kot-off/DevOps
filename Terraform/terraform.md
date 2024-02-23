# Terraform

## Установка

### бедитесь, что ваша система обновлена и у вас установлены пакеты gnupg, software-properties-common и curl. Вы будете использовать эти пакеты для проверки GPG-подписи HashiCorp и установки репозитория пакетов Debian от HashiCorp.

```
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
```

### Получаем релиз terraform

```
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg]
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
```

### Выполняем обновление apt и инсталируем terraform

```
sudo apt update && sudo apt install terraform
```

### Получаем GPG релиза terraform для использования в установке

```

```
