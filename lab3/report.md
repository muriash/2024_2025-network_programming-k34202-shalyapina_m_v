University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)

Year: 2024/2025

Group: K34202

Author: Shalyapina Maria Vasilievna

Lab: Lab2

Date of create: 18.10.2024

Date of finished: 18.10.2024

# Лабораторная работа №3 "Развертывание Netbox, сеть связи как источник правды в системе технического учета Netbox"

## Цель работы
С помощью Ansible и Netbox собрать всю возможную информацию об устройствах и сохранить их в отдельном файле.

## Ход выполнения работы

### Установка NetBox
PostgreSQL нужен для работы NetBox как основная реляционная база данных. Он используется для хранения всей структурированной информации

...

Redis в NetBox выполняет три ключевые функции: кэширование данных, асинхронная обработка задач и управление пользовательскими сессиями

...

Клонируем репозиторий NetBox

...

Создаем виртуальное окружение, устанавливаем необходимые зависимости и настраиваем конфигурационный файл configuration.py (копируем содержимое файла configuration_example.py), генерируем секретный ключ.

```bash
python3 -m venv venv
source venv/bin/activate
```
```bash
pip install -r requirements.txt
```
```bash
python3 generate_secret_key.py
```

Инициализируем базу данных и создаем суперпользователя
```bash
python3 manage.py migrate
python3 manage.py createsuperuser
```
Устанавливаем Gunicorn - это WSGI-сервер, который запускает Python-приложение NetBox и обрабатывает запросы, и Nginx - веб-сервер, который используется как обратный прокси для Gunicorn.
После этого запускам netbox и nginx

...

Пробрасываем нужные порты в VirtualBox, открываем браузер и переходим по ссылке https://localhost:8000

...

### Добавление информации об устройствах

Во вкладке "Устройства" была внесена информация о двух роутерах R1 и R2 (роль, производитель, тип, IP-адрес)

...

### Сохранение данных из NetBox в файл

Файл inventory.yaml:
```yaml
all:
  hosts:
    localhost:
      ansible_connection: local
```
Роль fetch_info (roles/fetch_info/tasks/main.yaml) для сбора данных и сохранения их в файл:
```yaml
---
- name: NetBox Connect
  uri:
    url: "{{ netbox_url }}/api/dcim/devices/"
    headers:
      Authorization: "Token {{ netbox_token }}"
    validate_certs: false
    return_content: true
  register: netbox_response

- name: Export Data
  copy:
    content: "{{ netbox_response.json | to_nice_json }}"
    dest: "{{ output_file }}"

```

Ansible плейбук playbook.yaml:
```yaml
---
- name: Gather devices info
  hosts: localhost
  gather_facts: false
  vars:
    netbox_url: "https://10.0.2.15:443"
    netbox_token: "b0e1baf3819699b9f81b68096e13a811377f8f7a"
    output_file: "netbox_devices.json"
  roles:
    - fetch_info
```

Запускаем выполнение плейбука командой
```bash
ansible-playbook -i inventory.yaml playbook.yaml
```

...

Получили файл netbox_devices.json, содержащий собранную информацию об устройствах













