---
## Front matter
lang: ru-RU
title: Лабораторная работа №15
subtitle: Управление логическими томами
author:
  - Наурузова Айшат Магометовна
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 21 ноября 2025

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

Получить навыки работы с физическими томами, группами томов и логическими томами в LVM.

# Ход выполнения

## Создание раздела /dev/sdb1

![Создание нового раздела](Screenshot_1.png){ width=70% }

## Создание физического тома

![Создание PV](Screenshot_2.png){ width=70% }

## Создание группы томов и логического тома

![Создание VG и LV](Screenshot_3.png){ width=70% }

## Добавление точки монтирования

![fstab](Screenshot_4.png){ width=70% }

## Проверка монтирования файловой системы

![Проверка монтирования](Screenshot_5.png){ width=70% }

## Создание нового раздела /dev/sdb2

![Создание второго раздела](Screenshot_6.png){ width=70% }

## Расширение группы томов vgdata

![Расширение VG](Screenshot_7.png){ width=70% }

## Увеличение логического тома lvdata

![Увеличение LV](Screenshot_8.png){ width=70% }

## Уменьшение логического тома

![Уменьшение LV](Screenshot_9.png){ width=70% }

# Самостоятельная работа

## Создание раздела на диске /dev/sdc

![Создание раздела](Screenshot_10.png){ width=70% }

## Создание PV, VG и LV

![Создание PV, VG и LV](Screenshot_11.png){ width=70% }

## Форматирование XFS

![Форматирование XFS](Screenshot_12.png){ width=70% }

## Добавление точки монтирования

![fstab](Screenshot_13.png){ width=70% }

## Проверка подключения файловой системы

![Проверка монтирования](Screenshot_14.png){ width=70% }

## Создание нового раздела /dev/sda2

![Создание раздела sda2](Screenshot_15.png){ width=70% }

## Увеличение LV и файловой системы XFS

![Расширение LV и FS](Screenshot_16.png){ width=70% }

## Проверка PV, VG и LV

![Проверка LV, VG, PV](Screenshot_17.png){ width=70% }

# Итоги работы

## Вывод

Получены практические навыки создания, расширения и уменьшения логических томов, а также управления файловыми системами EXT4 и XFS.
