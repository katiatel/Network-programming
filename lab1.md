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


<img width="428" alt="image" src="https://user-images.githubusercontent.com/53398280/199183723-e7865605-19c5-4179-8473-a80c66ac0b9c.png">

 



Устанавливаем MikroTik 7 версии в VirtualBox. После загрузки отключаем загрузочный диск вручную в настройках VirtualBox, поскольку этого не происходит происходит автоматически.


Теперь создадим WireGuard сервер на удаленной машине Ubuntu (VPN сервер).

•	Для установки используем команду - sudo apt install wireguard;

•	Прописываем в WireGuard файл условий интерфейса;

•	Создаем пару ssh ключей.


Поставили графический интерфейс WinBox. Подключаемся к виртуальной машине MikroTik.


<img width="327" alt="image" src="https://user-images.githubusercontent.com/53398280/199183886-4ac521e5-1b38-4d7b-848b-c237204598a8.png">



 Заходим в настройки Wireguard и создаем новый интерфейс и заполняем строки (ListenPort узнаём командой вывода файла /etc/wireguard/wg0.conf на сервере).

<img width="292" alt="image" src="https://user-images.githubusercontent.com/53398280/199183977-d159d2aa-2f29-4e18-9c11-39c670472bbc.png">


 
Открываем Wireguard Peer и указываем созданный на предыдущем шаге интерфейс (Interface), публичный ключ сервера (Public Key), публичный IP (Endpoint), порт (Endpoint Port), адрес, по которому будет идти подключение (Allowed Address).


<img width="229" alt="image" src="https://user-images.githubusercontent.com/53398280/199184045-cd8f2bfe-e870-4d30-8a1b-0d572f2e35f3.png">



Во вкладке Addresses указываем выбранную сеть (192.168.6.67/24) и интерфейс (wireguard1).

 
 <img width="306" alt="image" src="https://user-images.githubusercontent.com/53398280/199184103-54512001-3484-45e3-9d90-eb3cfc58d09e.png">

 

Во вкладке Firewall создаем NAT Rule masquerade, что позволит нам преобразовывать несколько IP-адресов в другой одиночный IP-адрес. 


<img width="293" alt="image" src="https://user-images.githubusercontent.com/53398280/199184151-387d83af-1574-42a0-9c78-288b69b4cbd0.png">



Теперь пингуем виртуальную машину по заданному IP адресу. Ping идет успешно, а значит все настройки выполнены правильно.


<img width="355" alt="image" src="https://user-images.githubusercontent.com/53398280/199198070-f3c1b3f0-b4a3-4a18-963d-b53a7a5d5c5c.png">



В результате получаем связь по следующей схеме. 


<img width="468" alt="image" src="https://user-images.githubusercontent.com/53398280/199184242-49cb2f9a-93a3-43b7-9409-4b265c593e37.png">


Вывод: в ходе выполнения лабораторной работы мы научились устанавливать систему Ansible, устанавливать Mikrotik RouterOS в VirtualBox, а также смогли создать VPN туннель между виртуальной машиной Ubuntu и удаленной виртуальной машиной при помощи WireGuard.

