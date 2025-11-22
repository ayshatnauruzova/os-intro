---
## Front matter
lang: ru-RU
title: Лабораторная работа №12
subtitle: Настройки сети в Linux
author:
  - Наурузова Айшат Магометовна
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 31 октября 2025

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

# Цель работы

## Основная цель

Получить навыки настройки сетевых параметров и управления подключениями в Linux с использованием утилит **ip**, **nmcli** и **nmtui**.

# Ход выполнения работы

## Проверка конфигурации сети

![Проверка интерфейсов](Screenshot_1.png){ #fig:001 width=70% }

## Добавление IP и сравнение с ifconfig

![Добавление IP-адреса и проверка портов](Screenshot_4.png){ #fig:002 width=70% }

## Управление подключениями с помощью nmcli

![Настройка соединений](Screenshot_7.png){ #fig:003 width=70% }

## Изменение параметров соединений

![Изменение параметров static](Screenshot_10.png){ #fig:004 width=70% }

## Проверка через nmtui

![Просмотр через nmtui](Screenshot_12.png){ #fig:005 width=70% }

# Итоги работы

## Вывод

В результате лабораторной работы освоены практические навыки конфигурации сети в Linux.  
Рассмотрены команды **ip**, **nmcli**, **nmtui**, а также принципы работы DHCP и статической адресации.  
Полученные знания позволяют уверенно управлять сетевыми параметрами и проводить диагностику сетевых подключений.
