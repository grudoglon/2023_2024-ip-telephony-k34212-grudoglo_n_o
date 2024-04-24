# Отчет по лабораторной работе №1 "Базовая настройка ip-телефонов в среде Сisco packet tracer"
University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [IP telephony technologies](https://itmo-ict-faculty.github.io/ip-telephony/)

Year: 2023/2024

Group: K34212

Author: Grudoglo Nikita Olegovich

Lab: Lab1

Date of create: 23.03.2024

Date of finished: 24.04.2024

## Цель работы: 

Изучить рабочую среду Cisco Packet Tracer, ознакомиться с интерфейсами основных устройств, типами кабелей, научиться собирать топологию. Изучить построение сети IP-телефонии с помощью маршрутизатора, коммутатора и IP телефонов Cisco 7960 в среде Packet tracer.

## Ход работы:

- ### Часть 1

В среде Cisco Packet Tracer была собрана схема соединения:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab_1/images/lab1_image1.jpg)

Интерфейсу каждого PC был задан ip-адрес 192.168.0.30-192.168.0.37. Маска 255.255.255.0  Пример PC0; PC3; PC7:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab_1/images/lab1_PC0.jpg)
![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab_1/images/lab1_PC3.jpg)
![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab_1/images/lab1_PC7.jpg)

Посредством пинга убедимся, что любой компьютер одной сети передает пакеты любому компьютеру другой сети.

Пинг с PC3 на PC7:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab_1/images/lab1_PC3-PC7.jpg)


- ### Часть 2

В среде Cisco Packet Tracer была собрана схема соединения, имя маршрутизатора было изменено на CMERouter:



Для работы IP-телефоны в разделе физического предcтавления подключим к источнику питания:



На маршрутизаторе поднимем интерфейс FastEthernet 0/0 и присвоим ему IP–адрес для работы сети:



Настроим  DHCP-сервер, выделим пул для IP–телефонов:

```
Router(config)#ip dhcp pool voice
Router(dhcp-config)#network 192.168.0.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.0.1
Router(dhcp-config)#option 150 ip 192.168.0.1
```

Далее на маршрутизаторе настроим VoIP параметры (максимально возможное количество поддерживаемых номеров и телефонов, присвоение линий в автоматическом режиме):

```
Router(config)#telephony-service
Router(config-telephony)#max-dn 15
Router(config-telephony)#max-ephones 15
Router(config-telephony)#ip source-address 192.168.0.1 port 3100
Router(config-telephony)#auto assign 1 to 19
```

На коммутаторе на подключенных интерфейсах включим поддержку VoIP (голосовой VLAN):

```
Switch>en
Switch#conf t
Switch(config)#interface FastEthernet 0/1
Switch(config-if)#switchport mode access
Switch(config-if)#switchport voice vlan 1
Switch(config-if)#interface range FastEthernet 0/23-24
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport voice vlan 1
```

```
Switch#conf t
Switch(config)#interface FastEthernet 0/3
Switch(config-if)#switchport mode access
Switch(config-if)#switchport voice vlan 1
Switch(config-if)#interface range FastEthernet 0/23-24
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport voice vlan 1
```

На маршрутизаторе присваиваем телефонам номера:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab_1/images/lab_1_2_phone_numbers.jpg)

На рисунке можно увидеть, что телефон получил IP-адрес:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab_1/images/lab1_2_phone_IP.jpg)

Проверим звонок между телефонами:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab_1/images/lab1_2_connetced_phones.jpg)


## Вывод:

При выполнении работы в среде Cisco Packet Tracer были построены две топологии сети. Были изучены базовый функционал Cisco Packet Tracer, настройка сети IP-телефонии.
