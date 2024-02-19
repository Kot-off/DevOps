# Ansible

## Установка [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

## Первоначальные настройки

### Создаем файл в /root/.ansible/ansible.cfg и записываем следующее

```
[defaults]
inventory = /root/.ansible/hosts
host_key_checking = false
```

- inventory = /root/.ansible/hosts - путь до файла в котором мы будем записывать информацию о клиентах. ip, pass и переменные.
- host_key_checking = false - для того, чтобы не проверялось рукопожатие - host_key

### Создаем файл hosts и прописываем следующее

```
wn1 ansible_host=192.168.ххх.8   ansible_user=root ansible_ssh_private_key_file=
wn2 ansible_host=192.168.ххх.10  ansible_user=root ansible_ssh_private_key_file=
```

### Создаем

```

```

### Создаем

```

```
