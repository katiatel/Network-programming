University: ITMO University

Faculty: FICT

Course: Network programming

Year: 2022

Group: K34212

Автор: Телегина Екатерина Олеговна

Lab: Lab1

Тема работы: Установка CHR и Ansible, настройка VPN
 
Цель работы: 

•	развернуть виртуальную машину с установленной системой контроля конфигураций Ansible;

•	установить Mikrotik RouterOS в VirtualBox;

•	установка VPN туннеля между машиной Ubuntu и RouterOS.


Ход работы:

Сначала создаем виртуальную машину Ubuntu на Яндекс Облаке. Затем устанавливаем python3 и Ansible с помощью команд:

•	sudo apt install python3-pip

•	sudo pip3 install ansible


Подключаемся к этой виртуальной машине по ssh ключу.

 <img width="327" alt="image" src="https://user-images.githubusercontent.com/53398280/199183058-015ad8ab-ecde-4f00-8a1b-7c37c52fa049.png">
 



Устанавливаем MikroTik 7 версии в VirtualBox. После загрузки отключаем загрузочный диск вручную в настройках VirtualBox, поскольку этого не происходит происходит автоматически.


Теперь создадим WireGuard сервер на удаленной машине Ubuntu (VPN сервер).

•	Для установки используем команду - sudo apt install wireguard;

•	Прописываем в WireGuard файл условий интерфейса;

•	Создаем пару ssh ключей.


Поставили графический интерфейс WinBox. Подключаемся к виртуальной машине MikroTik.

<img width="292" alt="image" src="https://user-images.githubusercontent.com/53398280/199183088-60f57c64-e034-43dd-885e-cae939789d17.png">


 

Заходим в настройки Wireguard и создаем новый интерфейс и заполняем строки (ListenPort узнаём командой вывода файла /etc/wireguard/wg0.conf на сервере).
<img width="229" alt="image" src="https://user-images.githubusercontent.com/53398280/199183115-cbd4ce5d-744c-42e0-bd59-6be786a12310.png">

 

Открываем Wireguard Peer и указываем созданный на предыдущем шаге интерфейс (Interface), публичный ключ сервера (Public Key), публичный IP (Endpoint), порт (Endpoint Port), адрес, по которому будет идти подключение (Allowed Address).

 <img width="306" alt="image" src="https://user-images.githubusercontent.com/53398280/199183137-f284e899-4fd7-442c-be8d-9fd5c95a5be6.png">


Во вкладке Addresses указываем выбранную сеть (192.168.6.67/24) и интерфейс (wireguard1).
 
 <img width="293" alt="image" src="https://user-images.githubusercontent.com/53398280/199183186-e76d6c4c-2bf0-423a-a7ef-4ebad2f6d7e8.png">


Во вкладке Firewall создаем NAT Rule masquerade, что позволит нам преобразовывать несколько IP-адресов в другой одиночный IP-адрес. 
 <img width="415" alt="image" src="https://user-images.githubusercontent.com/53398280/199183209-4e92d14d-6e7a-4122-a0ea-3cb66d8b3244.png">


Теперь пингуем виртуальную машину по заданному IP адресу. Ping идет успешно, а значит все настройки выполнены правильно.
 <img width="468" alt="image" src="https://user-images.githubusercontent.com/53398280/199183249-bdd23799-be7d-47c1-a697-9c796e7fd943.png">



В результате получаем связь по следующей схеме. 
![Uploading image.png…]()

 
Вывод: в ходе выполнения лабораторной работы мы научились устанавливать систему Ansible, устанавливать Mikrotik RouterOS в VirtualBox, а также смогли создать VPN туннель между виртуальной машиной Ubuntu и удаленной виртуальной машиной при помощи WireGuard.

