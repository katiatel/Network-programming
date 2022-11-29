Faculty: FICT

Course: Network programming

Year: 2022/2023

Group: K34212

Author: Telegina Ekaterina

Lab: Lab2

Цель работы: 
С помощью Ansible настроить несколько сетевых устройств и собрать информацию о них. 
Правильно собрать файл Inventory.

В VirtualBox создаем вторую виртуальную машину, аналогично тому, как мы это делали в первой лабораторной работе. 

После настройки пароля для пользователя admin можем зайти на веб-интерфейс по динамически выделенному для роутера IP-адресу. 

<img width="296" alt="image" src="https://user-images.githubusercontent.com/53398280/204485352-ad1e0031-c953-4451-95f0-349b4951cab4.png">

Для настройки OpenVPN Client на новом маршрутизаторе, генерируем для него сертификат и ключ на удаленном сервере, 
с помощью команд «sudo ./easyrsa gen-req и sudo» / «easyrsa sign-req client».

<img width="276" alt="image" src="https://user-images.githubusercontent.com/53398280/204485505-c05119e6-e06c-452b-b52d-d7433b760937.png">


Затем созданные ключ и сертификат подгружаем в раздел файлов на маршрутизаторе. 
Символы КТ в первой ячейке демонстрируют, что сертификат был проинициализирован и подтвержден ключом.

<img width="258" alt="image" src="https://user-images.githubusercontent.com/53398280/204485812-1bcf3e69-73a1-45d5-b111-ab2e1286ef42.png">

<img width="259" alt="image" src="https://user-images.githubusercontent.com/53398280/204485928-f1bde6f4-b334-4e7e-be3c-1d4b3efc9b8c.png">


 
В разделе РРР создаем новый OpenVPN клиент. 
Указываем публичный IP-адрес удаленного сервера, используемый сертификат, алгоритмы аутентификации и шифрования.
<img width="368" alt="image" src="https://user-images.githubusercontent.com/53398280/204486033-86252443-4480-4211-8c1a-4cf74db71c7e.png">


С помощью записей в разделе логов проводим проверку подключения. 

Соединение с сервером успешно установлено, ошибки не обнаружены.

OpenVPN сервер при подключении присвоил маршрутизатору еще один IP-адрес, через который устанавливается защищенное соединение между клиентом и сервером.

 <img width="417" alt="image" src="https://user-images.githubusercontent.com/53398280/204486122-86baf09b-9ead-4f3a-817c-3f86c08a939c.png">

С помощью пинга отправляем ICMP пакеты с сервера на клиенты, и с клиентов на сервер. 

Видим, что IP-связность клиентов с сервером установлена. 

 <img width="468" alt="image" src="https://user-images.githubusercontent.com/53398280/204486171-13ff332f-baae-4da0-ae6f-cff18f03696c.png">
 
<img width="468" alt="image" src="https://user-images.githubusercontent.com/53398280/204486205-ef6c89a6-ba60-4642-a6b5-702790f18230.png">

 Все хосты, с которыми будет работать Ansible указываем в файле в корневой папке пользователя «/ansible/etc/hosts». 
 
Хосты объединяем в группу «routers», каждому из хостов присваиваем инвентарное имя и переменная rid, которая понадобится для настройки OSPF.

<img width="368" alt="image" src="https://user-images.githubusercontent.com/53398280/204486265-8c25683e-3ede-4da9-9545-7a30777384d7.png">
 
Для проверки выполняем команду, выводящую информацию о хостах, которые содержатся в инвентарном файле.

<img width="468" alt="image" src="https://user-images.githubusercontent.com/53398280/204486301-37bad95c-08cf-4a0a-9065-ed6889629e81.png">
 
Ansible успешно выполнил команду ping на всех указанных хостах.

 <img width="468" alt="image" src="https://user-images.githubusercontent.com/53398280/204486359-f81ff2ff-62b4-44b8-bb97-9625c8c1225f.png">

Создаем плейбук (файл с указаниями для работы Ansible), в него добавляем два play для:

1.	Настройки роутеров 
2.	Получения с них информации. 
Большая часть тасок в play выполняется с помощью «routeros_command», например создание нового пользователя, настройка параметров для NTP клиента, настройка OSPF маршрутизации в сети между двумя роутерами. 
С помощью «routeros_facts» собираем информацию о конфигурации устройств. 
Так как результаты выполнения команд не сохраняются автоматически после выполнения таски, с помощью команды «register» они были занесены в переменные. 
Команда «copy» помещает заданный текст в указанный файл.

<img width="468" alt="image" src="https://user-images.githubusercontent.com/53398280/204486434-dee16305-ee66-4bf2-951d-c00c60ff74ba.png">


 Плейбук запускаем командой ansible-playbook и выполняем без сообщений об ошибке.
 
 <img width="468" alt="image" src="https://user-images.githubusercontent.com/53398280/204486480-00675a9b-5d84-4ba1-b2da-715bf5a719ce.png">


В результате работы плейбука в корневой папке пользователя были созданы два файла, содержащих собранную с роутеров информацию о конфигурации устройств.

 <img width="468" alt="image" src="https://user-images.githubusercontent.com/53398280/204486582-68c8c994-3b27-4b90-a725-cfc242cbda91.png">


Активируем NTP клиент, для которого указаны первичный и вторичный сервера для настройки точного времени. 
IP-адреса серверов были получены из DNS-имен 1.ru.pool.ntp.org и 2.ru.pool.ntp.org.
 
 <img width="425" alt="image" src="https://user-images.githubusercontent.com/53398280/204486664-92af4e5b-a2dd-4647-b628-2658246707e1.png">

Роутеры видят друг друга в качестве соседей в одной сети, а также то, что между ними установлена связность адресов.
 
 <img width="468" alt="image" src="https://user-images.githubusercontent.com/53398280/204486710-671372d6-fdaa-431d-a8b3-c5197008e3d8.png">
 
<img width="468" alt="image" src="https://user-images.githubusercontent.com/53398280/204486790-45998810-a115-4b3f-9a4c-e227a430fe0b.png">

<img width="468" alt="image" src="https://user-images.githubusercontent.com/53398280/204486829-a5fc7832-f860-4c04-a068-6611c0fe4255.png">

<img width="466" alt="image" src="https://user-images.githubusercontent.com/53398280/204486864-29bed75b-6043-49d7-b155-7024bc00f430.png">


 
Таким образом, итоговая схема сети между двумя роутерами и удаленным сервером в облаке выглядит следующим образом.

 <img width="334" alt="image" src="https://user-images.githubusercontent.com/53398280/204486923-53babeaa-17e9-4d5c-95cb-3274c96006ee.png">


Выводы: 
В ходе выполнения работы были настроены два роутера через Аnsibe с удаленного сервера. Можно сделать вывод, что использование Ansible крайне удобно для настройки одинаковых конфигураций.

