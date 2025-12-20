---
## Front matter
lang: ru-RU
title: Лабораторная работа №16
subtitle: Программный RAID
author:
  - Наурузова Айшат Магометовна
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 03 декабря 2025

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

Получение практических навыков создания, настройки и управления RAID-массивами в Linux  
(создание RAID 1, работа с hot spare, преобразование RAID 1 → RAID 5).

# Ход выполнения работы

## Проверка подключённых дисков

![Проверка дисков](Screenshot_1.png){ width=70% }

## Создание разделов на дисках

![Создание разделов](Screenshot_2.png){ width=70% }

## Проверка типа разделов

![Тип разделов](Screenshot_3.png){ width=70% }

## Изменение ID разделов (Linux RAID)

![Изменение ID](Screenshot_4.png){ width=70% }

## Создание массива RAID 1

![Создание RAID](Screenshot_5.png){ width=70% }

## Проверка состояния RAID 1

![Проверка RAID](Screenshot_6.png){ width=70% }

## Настройка монтирования и запись в fstab

![fstab](Screenshot_7.png){ width=70% }

## Имитация сбоя диска

![Сбой диска](Screenshot_8.png){ width=70% }

## Создание RAID 1 и добавление резервного диска

![RAID hot spare](Screenshot_9.png){ width=70% }

## Проверка состояния массива

![Состояние массива](Screenshot_10.png){ width=70% }

## Имитация сбоя и автоматическое восстановление

![Сбой и восстановление](Screenshot_11.png){ width=70% }

## Исходное состояние RAID 1

![Исходный RAID1](Screenshot_12.png){ width=70% }

## Проверка состояния перед изменением уровня

![Перед изменением](Screenshot_13.png){ width=70% }

## Преобразование в RAID 5

![Преобразование RAID](Screenshot_14.png){ width=70% }

## Масштабирование RAID 5 до трёх дисков

![RAID 5 (3 диска)](Screenshot_15.png){ width=70% }

# Итоги работы

## Вывод

В ходе выполнения работы были созданы и настроены массивы RAID 1 и RAID 5,  
изучены принципы их функционирования, восстановление после отказов и возможности изменения уровня массива.  
Была рассмотрена концепция горячего резерва и выполнено масштабирование RAID.  
Полученные навыки позволяют эффективно управлять программными RAID-массива в Linux.
