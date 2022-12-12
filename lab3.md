University: ITMO University

Faculty: FICT

Course: Network programming

Year: 2022/2023

Group: K34212

Author: Ekaterina Telegina

Lab: Lab3

Date created:

Date finished:

ЦЕЛЬ РАБОТЫ

С помощью Ansible и Netbox собрать всю возможную информацию об устройствах и сохранить их в отдельном файле.


ХОД РАБОТЫ

1. Поднять Netbox на дополнительной VM

Создана новая VM, которая соединена с VM на Яндексе через Wireguard туннель.


Поднять Netbox на новой VM.

![image](https://user-images.githubusercontent.com/53398280/207084267-2b8bb169-2dec-482f-8602-6b0604918397.png)

![image](https://user-images.githubusercontent.com/53398280/207084335-4a71bae4-a1ee-4709-b223-2368fb6bc2dd.png)


Используя браузер, подключить к Netbox по порту 8000.

![image](https://user-images.githubusercontent.com/53398280/207084400-036b9b1b-99f1-404c-a53a-6e4f99910f80.png)

2. Заполнить информацию о CHR в Netbox

Заполнена информация о первом CHR.

![image](https://user-images.githubusercontent.com/53398280/207084538-cfe60dcf-11b9-41a1-8976-244b43ff71ac.png)

![image](https://user-images.githubusercontent.com/53398280/207084604-64035f2d-9cd8-444f-97d9-68fb166939f1.png)

Заполнена информация о втором CHR.

![image](https://user-images.githubusercontent.com/53398280/207084708-0778b0d3-dfa9-4751-a001-82a91bc97147.png)

![image](https://user-images.githubusercontent.com/53398280/207084825-fe348398-0d14-43de-b28b-cd3cbe87ce31.png)

3. Сохранить данные с Netbox в отдельный файл, используя Ansible

Редактирован файл hosts, добавлен адрес по которому подключить к Netbox

![image](https://user-images.githubusercontent.com/53398280/207084897-efc2e4ce-2462-415c-9e32-85e71f961759.png)

Создан API токен для обращения к Netbox плейбуком.

![image](https://user-images.githubusercontent.com/53398280/207084966-eee3b2c1-9f03-47df-9ebe-54276eb934a5.png)

Плейбук, который собирает все данные из Netbox. Необходимо установить pynetbox для использования плагин nb_lookup.

![image](https://user-images.githubusercontent.com/53398280/207085129-82db2687-3c9f-41f7-9269-7351fdbd9f84.png)

Запущен плейбук.

![image](https://user-images.githubusercontent.com/53398280/207085264-3cb88fac-ce42-4bc3-a869-17e849c9063c.png)

4. На основе данных из Netbox настроить 2 CHR

Плейбук, который на основе собранных данных из Netbox изменит имя устроства и добавит IP адрес на 2-х CHR.

![image](https://user-images.githubusercontent.com/53398280/207085372-114878d1-ae26-49da-b09f-b31314875e0f.png)


Запущен плейбук.

![image](https://user-images.githubusercontent.com/53398280/207085423-ea88cad4-c939-4e65-a471-6d330abaf5ff.png)

![image](https://user-images.githubusercontent.com/53398280/207085462-4d03a335-3901-4bc6-94c9-97c83c4b872a.png)


5. Написать сценарий, позволяющий собрать серийный номер устройства и вносящий серийный номер в Netbox

Плейбук позволяющий собрать серийный номер устройства и вносящий серийный номер в Netbox.

![image](https://user-images.githubusercontent.com/53398280/207085611-79e552b6-8ce8-4e56-bbc2-7625c351512b.png)

Запущен плейбук.

![image](https://user-images.githubusercontent.com/53398280/207085647-e1793046-b0b4-4c9d-a6fb-3cfa7385488c.png)

Серийный номер в Netbox добавлен.

![image](https://user-images.githubusercontent.com/53398280/207085699-0efa390e-4379-4963-97e9-d1519446576d.png)

![image](https://user-images.githubusercontent.com/53398280/207090662-af45aabc-b756-41cc-9585-603e2ba3d55c.png)

РЕЗУЛЬТАТЫ

Получаем файл netbox_data.json с данными, собранными с Netbox.

Получаем связь по следующей схеме.

![image](https://user-images.githubusercontent.com/53398280/207090728-2f580770-32f1-44e7-89b6-510f4ec7c0c1.png)
