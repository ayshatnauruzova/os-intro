---
## Front matter
lang: ru-RU
title: Лабораторная работа №14
subtitle: Партиции, файловые системы, монтирование
author:
  - Наурузова Айшат Магометовна
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 12 ноября 2025

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

Получить навыки создания разделов на диске, форматирования файловых систем и их монтирования в Linux.

# Ход выполнения работы

## Создание разделов MBR с помощью fdisk

![Вывод команды fdisk -l](Screenshot_1.png){ #fig:001 width=70% }

## Создание основного раздела

![Создание основного раздела](Screenshot_3.png){ #fig:002 width=70% }

## Создание расширенного и логического разделов

![Создание расширенного и логического разделов](Screenshot_5.png){ #fig:003 width=70% }

## Создание раздела подкачки

![Создание раздела подкачки](Screenshot_7.png){ #fig:004 width=70% }

## Проверка и активация swap-раздела

![Активация swap-раздела](Screenshot_8.png){ #fig:005 width=70% }

## Создание GPT-разделов с помощью gdisk

![Создание раздела GPT](Screenshot_9.png){ #fig:006 width=70% }

## Проверка созданных разделов GPT

![Проверка раздела GPT](Screenshot_10.png){ #fig:007 width=70% }

## Форматирование разделов XFS и EXT4

![Создание файловых систем XFS и EXT4](Screenshot_11.png){ #fig:008 width=70% }

## Ручное монтирование файловой системы

![Ручное монтирование и отмонтирование](Screenshot_12.png){ #fig:009 width=70% }

## Монтирование раздела XFS через /etc/fstab

![Добавление записи в /etc/fstab](Screenshot_14.png){ #fig:010 width=70% }

## Проверка монтирования через fstab

![Проверка монтирования через fstab](Screenshot_15.png){ #fig:011 width=70% }

## Самостоятельная работа — GPT-разметка

![Создание двух GPT-разделов](Screenshot_16.png){ #fig:012 width=70% }

## Форматирование и настройка swap

![Создание файловой системы и раздела подкачки](Screenshot_17.png){ #fig:013 width=70% }

## Добавление разделов в /etc/fstab

![Редактирование файла /etc/fstab](Screenshot_18.png){ #fig:014 width=70% }

## Проверка монтирования и swap

![Проверка монтирования и swap](Screenshot_19.png){ #fig:015 width=70% }

# Итоги работы

## Вывод

В ходе лабораторной работы были изучены утилиты **fdisk** и **gdisk**,  
созданы разделы с разметкой **MBR** и **GPT**, отформатированы в **XFS** и **EXT4**,  
а также настроен и активирован раздел подкачки **swap**.  

Освоены команды **mkfs**, **mkswap**, **mount**, **blkid**,  
и механизм автоматического монтирования через файл **/etc/fstab**.  
