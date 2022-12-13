University: ITMO University

Faculty: FICT

Course: Network programming

Year: 2022/2023

Group: K34212

Author: Ekaterina Telegina

Lab: Lab2



ЦЕЛЬ РАБОТЫ

С помощью Ansible настроить несколько сетевых устройств и собрать информацию о них. Правильно собрать файл Inventory.


ХОД РАБОТЫ

1. Установка Wireguard туннели

Конфигурация сервера Wireguard на виртуальной машине в Яндексе

Сгенерирована и сохранена пара открытого и закрытого ключей для сервера.


![image](https://user-images.githubusercontent.com/53398280/207038907-0508a466-91e6-4917-a84d-cd81221c992d.png)


Редактирован файл конфигурации сервера Wireguard 

![image](https://user-images.githubusercontent.com/53398280/207039106-70f45b38-3fc5-474e-b186-05e4b9a11081.png)


Данная конфигурация дает серверу адрес 192.168.0.1 и позволяет серверу послушать на порт 51820 для входящих пакетов.

Запущен интерфейс Wireguard.


![image](https://user-images.githubusercontent.com/53398280/207057589-e6c852ed-6e57-4f12-bd73-8b359c6c277d.png)

Конфигурация клиента Wireguard на 2-х CHR

Добавлен интерфейс Wireguard и назначен ему IP адрес 192.168.0.2

![image](https://user-images.githubusercontent.com/53398280/207060433-4c847e8d-19df-48f2-ad96-6f0ed3de9ba3.png)

![image](https://user-images.githubusercontent.com/53398280/207061230-a04ec2ed-534d-479d-953c-a5f87448832e.png)

На CHR добавлен Wireguard пир, указав необходимые параметры сервера.

![image](https://user-images.githubusercontent.com/53398280/207061412-6f8cd979-2f8c-4d4a-b6d4-329baea6795e.png)

На сервере добавлен Wireguard пир, указав необходимые параметры клиента.

![image](https://user-images.githubusercontent.com/53398280/207061524-5fad38e7-5b55-44e1-b874-af932ef0991f.png)

Повторить процесс для второго CHR, которому назначен IP адрес 192.168.0.3


2. Настройка CHR при помощи Ansible

Проверка наличия Ansible.

![image](https://user-images.githubusercontent.com/53398280/207061766-7f41c441-60fa-4aea-a8d5-aca8c79c4b77.png)

Редактирован файл hosts, в котором указаны адресы двух CHR.

![image](https://user-images.githubusercontent.com/53398280/207062109-6f6a93d4-0ad5-4728-9f7b-26c088a792b0.png)

Создан плейбук, который добавит нового пользователей, настроит NTP сервер и настроит OSPF на двух CHR.

![image](https://user-images.githubusercontent.com/53398280/207066231-ee26826b-26da-4a05-9b33-9c2d56c2f20b.png)

Запущен плейбук.

![image](https://user-images.githubusercontent.com/53398280/207066172-a0044373-c2a1-4871-884a-ea9cea0338c5.png)

Создан плейбук, который собирает данные по OSPF топологии и полный конфиг двух CHR. Конфиг устройства сохранен в отдельные файлы

![image](https://user-images.githubusercontent.com/53398280/207066345-5efee6a0-5694-4b7a-b0ad-5cb0e2e27213.png)

Запущен плейбук.

![image](https://user-images.githubusercontent.com/53398280/207066408-87173117-de61-4575-9cfe-0a5d049e326a.png)

3. Результаты

В результате получаем 2 файла с конфигурациями устройств и связь по следующей схеме.

![image](https://user-images.githubusercontent.com/53398280/207066536-4715945c-248f-4501-b7d0-af20bb0101c1.png)
