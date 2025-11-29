---
## Front matter
title: "Отчёт по лабораторной работе №13"
subtitle: "Фильтр пакетов"
author: "Наурузова Айшат Магометовна"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true
toc-depth: 2
lof: true
lot: true
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
    - spelling=modern
    - babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: IBM Plex Serif
romanfont: IBM Plex Serif
sansfont: IBM Plex Sans
monofont: IBM Plex Mono
mathfont: STIX Two Math
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float}
  - \floatplacement{figure}{H}
---

# Цель работы

Получить навыки настройки пакетного фильтра в Linux.

# Ход выполнения

## Управление брандмауэром с помощью `firewall-cmd`

После входа в систему выполнен переход к учётной записи **root**: `su -`.

### Определение активной зоны и доступных сервисов

- Текущая зона по умолчанию: `firewall-cmd --get-default-zone` → **public**.  
- Список зон: `firewall-cmd --get-zones`.  
- Доступные службы: `firewall-cmd --get-services`.  
- Разрешённые службы в активной зоне: `firewall-cmd --list-services`.  
- Подробная конфигурация зоны: `firewall-cmd --list-all`.  
- То же, но с явным указанием зоны: `firewall-cmd --list-all --zone=public` (выводы совпадают, так как активна **public**).

![Определение зоны и сервисов](Screenshot_1.png){ width=80% }
![Сравнение `--list-all` и `--list-all --zone=public`](Screenshot_2.png){ width=80% }

### Добавление службы VNC

- Добавлена служба: `firewall-cmd --add-service=vnc-server`.  
- Проверка: `firewall-cmd --list-all` — **vnc-server** присутствует.  
- Перезапуск демона: `systemctl restart firewalld`.  
- Повторная проверка: `firewall-cmd --list-all` — **vnc-server** исчез.

**Почему так произошло:** команда без флага `--permanent` меняет только **runtime-конфигурацию**. При перезапуске `firewalld` runtime очищается и берётся конфигурация с диска (permanent), где `vnc-server` ещё не был сохранён.

![Добавление и потеря vnc-server после перезапуска](Screenshot_3.png){ width=80% }

### Добавление VNC на постоянной основе

- Запись в конфигурацию на диск: `firewall-cmd --add-service=vnc-server --permanent`.  
- Немедленно в `--list-all` **vnc-server** не виден, т.к. изменена только permanent-конфигурация.  
- Применение: `firewall-cmd --reload`.  
- Проверка: `firewall-cmd --list-all` — **vnc-server** присутствует.

![Постоянное добавление vnc-server и применение через reload](Screenshot_4.png){ width=80% }

### Открытие порта 2022/TCP

- Добавление: `firewall-cmd --add-port=2022/tcp --permanent`.  
- Применение: `firewall-cmd --reload`.  
- Проверка: `firewall-cmd --list-all` — в разделе `ports` появилась запись `2022/tcp`.

![Открыт порт 2022/tcp](Screenshot_5.png){ width=80% }

## Управление через графический интерфейс `firewall-config`

1. Запуск: `firewall-config`. В выпадающем списке **Configuration** выбран режим **Permanent**, зона — **public**.  
2. На вкладке **Services** отмечены службы `http`, `https`, `ftp`.  
3. На вкладке **Ports** добавлен порт `2022` с протоколом `udp` (кнопка **Add** → OK).  
4. В терминале `firewall-cmd --list-all` — изменений нет (они пока только в permanent).  
5. Применение: `firewall-cmd --reload`.  
6. Проверка: `firewall-cmd --list-all` — службы `ftp http https` и порты `2022/tcp 2022/udp` активны.

![Включение http/https/ftp в GUI](Screenshot_6.png){ width=80% }
![Добавление UDP-порта 2022 в GUI](Screenshot_7.png){ width=80% }
![Изменения после reload (сервисы и порты применены)](Screenshot_8.png){ width=80% }

## Самостоятельная работа

1. **CLI (telnet):** `firewall-cmd --add-service=telnet --permanent` → `firewall-cmd --reload` → проверка `firewall-cmd --list-all`.  
2. **GUI (imap, pop3, smtp):** в `firewall-config` (режим **Permanent**, зона **public**) включены службы `imap`, `pop3`, `smtp`, затем `firewall-cmd --reload`.  
3. Контрольная проверка показала наличие `telnet imap pop3 smtp` среди разрешённых служб.

![Финальная конфигурация с telnet, imap, pop3, smtp](Screenshot_9.png){ width=80% }

# Контрольные вопросы

**1. Какая служба должна быть запущена перед началом работы с менеджером конфигурации брандмауэра firewall-config?**  
Перед использованием `firewall-config` должна быть запущена служба **firewalld**.

**2. Какая команда позволяет добавить UDP-порт 2355 в конфигурацию брандмауэра в зоне по умолчанию?**  
`firewall-cmd --add-port=2355/udp --permanent`

**3. Какая команда позволяет показать всю конфигурацию брандмауэра во всех зонах?**  
`firewall-cmd --list-all-zones`

**4. Какая команда позволяет удалить службу vnc-server из текущей конфигурации брандмауэра?**  
`firewall-cmd --remove-service=vnc-server`

**5. Какая команда firewall-cmd позволяет активировать новую конфигурацию, добавленную опцией --permanent?**  
`firewall-cmd --reload`

**6. Какой параметр firewall-cmd позволяет проверить, что новая конфигурация была добавлена в текущую зону и теперь активна?**  
`firewall-cmd --list-all`

**7. Какая команда позволяет добавить интерфейс eno1 в зону public?**  
`firewall-cmd --zone=public --add-interface=eno1`

**8. Если добавить новый интерфейс в конфигурацию брандмауэра, пока не указана зона, в какую зону он будет добавлен?**  
Он будет добавлен в **зону по умолчанию** (обычно это `public`).

# Заключение

В ходе выполнения работы было рассмотрено управление сетевыми правилами с помощью утилит `firewall-cmd` и `firewall-config`. Изучены различия между конфигурацией времени выполнения и постоянной конфигурацией, а также порядок применения изменений. На практике были добавлены сетевые службы и порты, выполнено их сохранение и проверка, а также проведено управление настройками через графический интерфейс. В результате освоены основные принципы администрирования межсетевого экрана в Linux, что позволяет гибко настраивать сетевой доступ и повышать уровень безопасности системы.
