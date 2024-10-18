University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)

Year: 2024/2025

Group: K34202

Author: Shalyapina Maria Vasilievna

Lab: Lab2

Date of create: 18.10.2024

Date of finished: 18.10.2024

# Лабораторная работа №2 "Развертывание дополнительного CHR, первый сценарий Ansible"

## Цель работы
С помощью Ansible настроить несколько сетевых устройств и собрать информацию о них. Правильно собрать файл Inventory.

## Ход выполнения работы

1. В VirtualBox был создан второй CHR
 
![1](https://github.com/user-attachments/assets/ee41fa42-d199-4788-afb8-1c3cf7b3c965)

2. На созданном роутере CHR2 был настроен Wireguard интерфейс

![2](https://github.com/user-attachments/assets/7ac31a05-0566-4958-b5c6-2b8b0ef05928)

3. На сервере в файл wg0.conf была добавлена информация о новом клиенте с адресом 10.66.66.3. Проверка доступности сервера со второго CHR:

![3](https://github.com/user-attachments/assets/a0601346-47b7-4b48-bbeb-f9614d007975)

4. Был создан файл inventory.yaml, содержащий информацию об узлах, на которых следует производить настройку

![wg интерфейс](https://github.com/user-attachments/assets/2ee7fbe9-b616-4995-818d-3f3942ca80e4)

5. Для настройки устройств был создан Ansible плейбук chr-playbook.yaml, который включает в себя следующие указания:
   - Set logpass: устанавливает пароль "admin" для пользователя admin
   - Configure NTP: включает и настраивает NTP клиента
   - Configure OSPF: настраивает протокол OSPF с указанием Router ID
   - Gather OSPF topology: собирает информацию по OSPF топологии
   - Print OSPF topology: выводит информацию по OSPF топологии
   - Gather full config: собирает все данные о конфигурации устройства
   - Print full config: выводит данные о конфигурации устройства

![конфиг](https://github.com/user-attachments/assets/3ee9222c-eb77-48c4-a2a4-0993431ab3f4)

6. Выполнение плейбука

![запуск](https://github.com/user-attachments/assets/ace12361-a9c4-4db8-ac99-0b9ca71cbb10)

7. Данные по OSPF топологии

```bash
TASK [Print OSPF topology] ******************************************************************************************************************************************************************
ok: [chr1] => {
    "ospf_topology.stdout_lines": [
        [
            "Flags: V - virtual; D - dynamic ",
            " 0  D instance=default area=backbone address=172.20.10.2 router-id=2.2.2.2 ",
            "      state=\"Full\" state-changes=6 adjacency=2m27s timeout=33s"
        ]
    ]
}
ok: [chr2] => {
    "ospf_topology.stdout_lines": [
        [
            "Flags: V - virtual; D - dynamic ",
            " 0  D instance=default area=backbone address=172.20.10.5 router-id=1.1.1.1 ",
            "      state=\"Full\" state-changes=5 adjacency=2m27s timeout=33s"
        ]
    ]
}
```

8. Полный конфиг устройств

![wg адрес](https://github.com/user-attachments/assets/5b4c387e-79fe-468e-93f2-aa590e31aee1)

## Результат

Проверка доступности сервера

![пинг сервера](https://github.com/user-attachments/assets/3e8a2659-21b8-422a-96a1-f329c3825791)

Проверка доступности клиента

![пинг клиента](https://github.com/user-attachments/assets/a87c4a1a-8915-42cd-a5ac-18dd6eab1d75)
