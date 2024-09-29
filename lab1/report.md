University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)

Year: 2024/2025

Group: K34202

Author: Shalyapina Maria Vasilievna

Lab: Lab1

Date of create: 28.09.2024

Date of finished: 29.09.2024

# Лабораторная работа №1 "Установка CHR и Ansible, настройка VPN"

## Описание
Данная работа предусматривает обучение развертыванию виртуальных машин (VM) и системы контроля конфигураций Ansible а также организации собственных VPN серверов.

## Цель работы
Целью данной работы является развертывание виртуальной машины на базе платформы Microsoft Azure с установленной системой контроля конфигураций Ansible и установка CHR в VirtualBox.

## Ход выполнения работы

1. Создание виртуальной машины (была выбрана Ubuntu 22.04), установка python3 и Ansible
 
![1](https://github.com/user-attachments/assets/64264674-0e2d-4cf8-b6ea-3f9ec9eb666f)

2. На другой виртуальной машине был установлен CHR

![микротик](https://github.com/user-attachments/assets/9a4813f2-8427-40fc-9605-d90ba4969aa8)

3. На сервере был установлен Wireguard, а также сгенерированы приватный и публичный ключи для сервера

![ключи](https://github.com/user-attachments/assets/1e90f781-03c9-4ca3-987d-bc914ff83f64)

4. Для настройки клиента было выполнено подключение к роутеру через WinBox, создан интерфейс WireGuard

![wg интерфейс](https://github.com/user-attachments/assets/2ee7fbe9-b616-4995-818d-3f3942ca80e4)

5. На сервере был прописан конфигурационный файл wg0.conf с публичным ключом клиента, полученным на предыдущем шаге

![конфиг](https://github.com/user-attachments/assets/3ee9222c-eb77-48c4-a2a4-0993431ab3f4)

6. Запускаем службу

![запуск](https://github.com/user-attachments/assets/ace12361-a9c4-4db8-ac99-0b9ca71cbb10)

7. Далее на CHR в разделе WireGuard -> Peers была добавлена информация о VPN-севере

![peer](https://github.com/user-attachments/assets/ebc9dd29-e47e-4725-aefd-e04a255fed72)

8. Созданному раннее интерфейсу wg-client был присвоен адрес 10.66.66.2 

![wg адрес](https://github.com/user-attachments/assets/5b4c387e-79fe-468e-93f2-aa590e31aee1)

## Результат

Проверка доступности сервера

![пинг сервера](https://github.com/user-attachments/assets/3e8a2659-21b8-422a-96a1-f329c3825791)

Проверка доступности клиента

![пинг клиента](https://github.com/user-attachments/assets/a87c4a1a-8915-42cd-a5ac-18dd6eab1d75)

