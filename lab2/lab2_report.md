
# Отчет по лабораторной работе №2 "Конфигурация voip в среде Сisco packet tracer"
University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [IP telephony technologies](https://itmo-ict-faculty.github.io/ip-telephony/)

Year: 2023/2024

Group: K34212

Author: Grudoglo Nikita Olegovich

Lab: Lab2

Date of create: 28.03.2024

Date of finished: 24.04.2024

## Цель работы: 

Изучить построение сети IP-телефонии с помощью маршрутизатора Cisco 2811, коммутатора Cisco catalyst 3560 и IP телефонов Cisco 7960.

## Ход работы:

### Часть 1

В среде Cisco Packet Tracer была собрана схема соединения:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab2/images/lab2_part1.jpg)

В конфигурационном режиме название маршрутизатора было изменено на CMERouter

Для работы IP-телефоны в разделе физического предcтавления подключим к источнику питания.

На маршрутизаторе был отключен синтаксис ввода слов от DNS серверов и заданы пароли для защиты маршрутизатора в удаленном режиме и в режиме консоли:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab2/images/lab2_part1_2.jpg)

На маршрутизаторе поднимем интерфейс FastEthernet 0/0 и присвоим ему IP–адрес:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab2/images/lab2_part1_3.jpg)

Также на маршрутизаторе настроим DHCP-сервер, выделим пул для IP–телефонов:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab2/images/lab2_part1_4.jpg)

На маршрутизаторе настроим VoIP параметры (максимально возможное количество поддерживаемых номеров и телефонов, присвоение линий в автоматическом режиме):

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab2/images/lab2_part1_5.jpg)

На коммутаторе на подключенных интерфейсах включаем поддержку VoIP (голосовой VLAN):

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab2/images/lab2_part1_6.jpg)

На маршрутизаторе присваиваем телефонам номера:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab2/images/lab2_part1_7.jpg)

Проверим звонок между телефонами:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab2/images/lab2_part1_8.jpg)

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab2/images/lab2_part1_9.jpg)

### Часть 2

В среде Cisco Packet Tracer была собрана схема соединения:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab2/images/lab2_part2.jpg)

Для работы IP-телефоны были подключены к источнику питания в разделе физического предcтавления.

На коммутаторе были добавлены 2 VLAN для передачи данных и голоса.

Интерфейс коммутатора, подключенный к роутеру, был переведен в режим trunk, была разрешена передача добавленных VLAN:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab2/images/lab2_part2_1.jpg)

Интерфейсы коммутатора, подключенные к ip-телефонам, были переведены в режим access для vlan 10 и vlan 20 в качестве голосового:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab2/images/lab2_part2_2.jpg)

На маршрутизаторе были подняты саб-интерфейсы, соответсвующие vlan, им были назначены IP–адреса:

```
Router(config-subif)#interface FastEthernet0/0
Router(config-subif)#no shutdown
Router(config-subif)#interface FastEthernet0/0.10
Router(config-subif)#encapsulation dot1Q 10
Router(config-subif)#ip address 192.168.10.1 255.255.255.0
Router(config-subif)#interface FastEthernet0/0.20
Router(config-subif)#encapsulation dot1Q 20
Router(config-subif)#ip address 192.168.20.1 255.255.255.0
```

Настройка на маршрутизаторе DHCP-сервер, выделим пулы для ПК и IP–телефонов:

```
Router(config)#ip dhcp pool data
Router(dhcp-config)#network 192.168.10.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.10.1
Router(dhcp-config)#ip dhcp pool voice
Router(dhcp-config)#network 192.168.20.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.20.1
Router(dhcp-config)#option 150 ip 192.168.20.1
```

На компьютерах необходимо включить получение ip-адреса по DHCP вместо static.

Далее на маршрутизаторе настроим VoIP параметры:

```
Router(config)#telephony-service
Router(config-telephony)#max-dn 15
Router(config-telephony)#max-ephones 15
Router(config-telephony)#ip source-address 192.168.20.1 port 3100
Router(config-telephony)#auto assign 1 to 15
```

Присвоим телефонам номера:

```
CMERouter(config)#ephone-dn 1
CMERouter(config-ephone-dn)#number 1111
CMERouter(config-ephone-dn)#exit
CMERouter(config)#ephone-dn 2
CMERouter(config-ephone-dn)#number 2222
CMERouter(config-ephone-dn)#exit
CMERouter(config)#ephone-dn 3
CMERouter(config-ephone-dn)#number 3333
```

Проверим звонок между телефонами:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab2/images/lab2_part2_3.jpg)

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab2/images/lab2_part2_4.jpg)



## Вывод:

При выполнении работы в среде Cisco Packet Tracer были построены две топологии сети. Была получены навыки по построению сетей IP-телефонии.
