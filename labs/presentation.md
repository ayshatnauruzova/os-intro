---
## Front matter
lang: ru-RU
title: Лабораторная работа №11
subtitle: Управление загрузкой системы
author:
  - Наурузова Айшат Магометовна
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 26 октября 2025

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

Получение практических навыков работы с загрузчиком GRUB2, изменение его параметров и восстановление системы при неполадках загрузки.

# Ход выполнения

## Модификация параметров GRUB2

![Редактирование файла /etc/default/grub](Screenshot_1.png){ #fig:001 width=70% }

## Применение конфигурации GRUB2

![Создание новой конфигурации GRUB2](Screenshot_2.png){ #fig:002 width=70% }

## Проверка отображения меню загрузки

![Меню загрузки GRUB после изменений](Screenshot_3.png){ #fig:003 width=70% }

## Режим восстановления (rescue.target)

![Редактирование параметров загрузки — режим восстановления](Screenshot_4.png){ #fig:004 width=70% }

## Проверка активных модулей в rescue

![Вывод активных модулей в режиме rescue](Screenshot_5.png){ #fig:005 width=70% }

## Просмотр переменных среды

![Переменные среды в режиме rescue](Screenshot_5.png){ #fig:006 width=70% }

## Аварийный режим (emergency.target)

![Редактирование строки для аварийного режима](Screenshot_6.png){ #fig:007 width=70% }

## Минимальная загрузка системы

![Список активных модулей в emergency.target](Screenshot_7.png){ #fig:008 width=70% }

## Сброс пароля root (rd.break)

![Редактирование строки загрузки с параметром rd.break](Screenshot_8.png){ #fig:009 width=70% }

## Попытка монтирования и chroot

![Режим rd.break — монтирование и попытка chroot](Screenshot_9.png){ #fig:010 width=70% }

# Итоги работы

## Вывод

В ходе работы были изучены принципы настройки и модификации параметров загрузчика GRUB2.  
Отработаны действия по изменению таймаута меню, переходу в режимы восстановления и аварийной загрузки, а также выполнены шаги по сбросу пароля root.  
Полученные навыки позволяют администратору эффективно управлять процессом загрузки и устранять ошибки при запуске системы.
