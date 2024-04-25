# Отчет по лабораторной работе №3 "Использование Asterisk в качестве SIP proxy"
University: ITMO University (https://itmo.ru/ru/)

Faculty: FICT (https://fict.itmo.ru/)

Course: IP telephony technologies (https://itmo-ict-faculty.github.io/ip-telephony/)

Year: 2023/2024

Group: K34212

Author: Grudoglo Nikita Olegovich

Lab: Lab3

Date of create: 24.04.2024

Date of finished: 25.04.2024

## Цель работы: 

Изучить программный комплекс Asterisk, настроить Asterisk для локальных звонков.

## Ход работы:

### Установка и настройка Asterisk
Работа выполняется на машине с ОС Ubuntu
Устанавливаем Asterisk:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab_3/images/lab3_1.jpg)

Для добавления двух SIP-аккаунтов (1001 и 1002) и маршрутизации внутренних вызовов в файл /etc/asterisk/sip.conf было добавлено:

```
[1001]
type=friend
host=dynamic
secret=111
context=ext_1001

[1002]
type=friend
host=dynamic
secret=222
context=ext_1002

[internal_calls]
exten => 1001,1,Dial(SIP/1002)
exten => 1002,1,Dial(SIP/1001)

[default]
include => internal_calls
```
![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab_3/images/lab3_2.jpg)

Также была добавлена информация в файл /etc/asterisk/extensions.conf:

```
[ext_1001]
exten => _XXXX,1,Dial(SIP/${EXTEN})

[ext_1002]
exten => _XXXX,1,Dial(SIP/${EXTEN})
```
![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab_3/images/lab3_3.jpg)

Для применения настроек перезагружаем Asterisk и проверяем наличие аккаунтов:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab_3/images/lab3_4.jpg)

### Установка Zoiper

Установка Zoiper5 и добавление аккаунта 1001:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab_3/images/lab3_5.jpg)

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab_3/images/lab3_6.jpg)


Аккаунт 1001 получил свой порт:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab_3/images/lab3_7.jpg)

### Установка MicroSIP

В качестве второго soft-телефона был установлен MicroSIP (с помощью предварительно установленного wine).
Здесь были введены данные аккаунта 1002, в качестве сервера так же был указан локальный хост (127.0.0.1).:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab_3/images/lab3_8.jpg)

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab_3/images/lab3_9.jpg)

Аккаунт 1002 также получил свой порт:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab_3/images/lab3_10.jpg)

### Звонок

Далее с 1002 был совершен звонок на 1001:

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab_3/images/lab3_11.jpg)

![](https://github.com/grudoglon/2023_2024-ip-telephony-k34212-grudoglo_n_o/blob/main/lab_3/images/lab3_12.jpg)

## Вывод:

При выполнении работы был изучен и настроен для локальных звонков программный комплекс Asterisk, был проведен звонок между двумя SIP-аккаунтами с помощью soft-телефонов Zoiper и MicroSIP.
