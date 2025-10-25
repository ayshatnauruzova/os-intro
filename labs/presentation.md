---
## Front matter
lang: ru-RU
title: Лабораторная работа №8
subtitle: Планировщики событий
author:
  - Наурузова Айшат Магометовна
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 10 октября 2025

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

Получение навыков работы с планировщиками событий cron и at в операционной системе Linux.

# Ход выполнения работы

## Проверка службы cron

![Проверка статуса службы crond](Screenshot_1.png){ #fig:001 width=70% }

## Просмотр и настройка crontab

![Создание задания в crontab](Screenshot_2.png){ #fig:002 width=70% }

## Проверка выполнения задания cron

![Проверка выполнения задания cron](Screenshot_3.png){ #fig:003 width=70% }

## Изменение расписания cron

![Редактирование расписания cron](Screenshot_4.png){ #fig:004 width=70% }

## Использование каталогов cron.hourly и cron.d

![Создание задания в /etc/cron.d](Screenshot_6.png){ #fig:006 width=70% }

## Проверка работы службы atd

![Проверка службы atd](Screenshot_7.png){ #fig:007 width=70% }

# Итоги работы

## Вывод

В ходе лабораторной работы были изучены инструменты планирования задач cron и at.  
Созданы и проверены периодические и разовые задания, освоен синтаксис crontab и использование каталогов cron.  
Изучены механизмы контроля доступа и гарантированного выполнения заданий с помощью anacron.  
Полученные знания позволяют эффективно автоматизировать системные процессы в Linux.
