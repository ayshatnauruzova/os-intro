---
## Front matter
lang: ru-RU
title: Лабораторная работа №13
subtitle: Фильтр пакетов (firewalld)
author:
  - Наурузова Айшат Магометовна
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 05 ноября 2025

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

Получить практические навыки настройки брандмауэра  в Linux с помощью `firewall-cmd` и `firewall-config`.

# Ход выполнения

## Определение активной зоны и доступных сервисов

![Определение зоны и сервисов](Screenshot_1.png){ width=70% }

## Детальный вывод конфигурации зоны

![Сравнение list-all и указания зоны](Screenshot_2.png){ width=70% }

## Добавление службы VNC (runtime)

![Добавление VNC и потеря после рестарта](Screenshot_3.png){ width=70% }

## Добавление VNC в постоянную конфигурацию

![Добавление vnc permanent](Screenshot_4.png){ width=70% }

## Открытие порта 2022/TCP

![Добавление порта](Screenshot_5.png){ width=70% }

## firewall-config — включение сервисов

![Добавление сервисов в GUI](Screenshot_6.png){ width=70% }

## Добавление порта в GUI

![Добавление UDP порта](Screenshot_7.png){ width=70% }

## Применение конфигурации

![Изменения применены](Screenshot_8.png){ width=70% }

## Добавление служб

![Финальная конфигурация](Screenshot_9.png){ width=70% }

# Итоги работы

## Вывод

- Освоена работа с `firewall-cmd` (CLI) и `firewall-config` (GUI)
- Изучены **runtime** и **permanent** конфигурации
- Выполнено добавление сервисов и портов
- Получены практические навыки управления сетевой безопасностью

Настройка брандмауэра позволяет контролировать сетевой доступ и повышать безопасность системы Linux.
