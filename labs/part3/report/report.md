---
## Front matter
title: "Отчёт по лабораторной работе №11"
subtitle: "Управление загрузкой системы"
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

Получить навыки работы с загрузчиком системы GRUB2.

# Ход выполнения

## Модификация параметров GRUB2

Для выполнения лабораторной работы был открыт терминал и получены административные права командой `su -`.  

Затем был отредактирован файл конфигурации GRUB — `/etc/default/grub`.  
В нём изменён параметр времени отображения меню загрузки с 20 на 10 секунд:  
`GRUB_TIMEOUT=10`.  

После сохранения изменений и закрытия редактора был выполнен просмотр содержимого файла.  

![Редактирование файла /etc/default/grub](Screenshot_1.png){ #fig:001 width=70% }

Для применения настроек была выполнена команда генерации новой конфигурации загрузчика:  
`grub2-mkconfig > /boot/grub2/grub.cfg`.  

Система вывела сообщение о создании файла конфигурации и добавлении записи для UEFI Firmware Settings.  

![Создание новой конфигурации GRUB2](Screenshot_2.png){ #fig:002 width=70% }

После перезагрузки в меню GRUB отобразились доступные варианты загрузки операционной системы.  

![Меню загрузки GRUB после изменений](Screenshot_3.png){ #fig:003 width=70% }

## Загрузка в режиме восстановления (rescue.target)

После появления меню GRUB была выбрана текущая версия ядра, затем нажата клавиша **e** для редактирования параметров.  
В конце строки, начинающейся с `linux ($root)/vmlinuz-`, был добавлен параметр `systemd.unit=rescue.target`, а опции `rhgb` и `quiet` были удалены.  

![Редактирование параметров загрузки — режим восстановления](Screenshot_4.png){ #fig:004 width=70% }

После загрузки в режиме восстановления был выполнен просмотр активных модулей командой `systemctl list-units`.  
Отобразился список базовых системных служб, необходимых для функционирования минимальной среды.  

![Вывод активных модулей в режиме rescue](Screenshot_5.png){ #fig:005 width=70% }

Для проверки переменных среды была использована команда `systemctl show-environment`, что подтвердило успешную инициализацию минимальной среды.  

## Загрузка в аварийном режиме (emergency.target)

Система была перезагружена, после чего снова выбрана текущая версия ядра и нажата клавиша **e**.  
В конце строки, загружающей ядро, был добавлен параметр `systemd.unit=emergency.target`, а параметры `rhgb` и `quiet` удалены.  

![Редактирование строки для аварийного режима](Screenshot_6.png){ #fig:006 width=70% }

После загрузки в аварийном режиме был выполнен просмотр списка активных модулей с помощью `systemctl list-units`.  
Количество загруженных модулей сократилось до минимального, необходимых для экстренного восстановления.  

![Список активных модулей в emergency.target](Screenshot_7.png){ #fig:007 width=70% }

## Сброс пароля root

Для сброса пароля root система была перезагружена, затем в меню GRUB выбран текущий пункт ядра и нажата клавиша **e**.  
В конце строки, загружающей ядро, был добавлен параметр `rd.break`, а опции `rhgb` и `quiet` удалены.  

![Редактирование строки загрузки с параметром rd.break](Screenshot_8.png){ #fig:008 width=70% }

После нажатия **Ctrl + X** система перешла в среду `initramfs`, при этом процесс загрузки был остановлен до монтирования корневой файловой системы.  
Для получения доступа к системному разделу были введены команды `mount -o remount,rw /sysroot` и `chroot /sysroot`.  

Команда `mount` успешно выполнилась, однако попытка использования `chroot` завершилась сообщением об ошибке: `command not found`.  

![Режим rd.break — монтирование и попытка chroot](Screenshot_9.png){ #fig:009 width=70% }

# Контрольные вопросы

**1. Какой файл конфигурации следует изменить для применения общих изменений в GRUB2?**  
Необходимо отредактировать файл `/etc/default/grub`, который содержит основные параметры загрузчика GRUB2.  

**2. Как называется конфигурационный файл GRUB2, в котором вы применяете изменения для GRUB2?**  
Основной конфигурационный файл GRUB2 — это `/boot/grub2/grub.cfg`.  
Именно он используется при загрузке системы для чтения всех установленных параметров.  

**3. После внесения изменений в конфигурацию GRUB2, какую команду вы должны выполнить, чтобы изменения сохранились и воспринялись при загрузке системы?**  
После редактирования конфигурации необходимо сгенерировать новый файл параметров с помощью команды:  
`grub2-mkconfig > /boot/grub2/grub.cfg`  
Эта команда пересоздаёт актуальную конфигурацию GRUB2 на основе настроек из `/etc/default/grub`.  

# Заключение

В ходе работы были изучены основные методы настройки и модификации параметров загрузчика GRUB2.  
Были рассмотрены способы изменения времени отображения меню, редактирования параметров загрузки ядра, а также процедуры перехода в режимы восстановления и аварийной загрузки.  
Полученные навыки позволяют эффективно управлять процессом загрузки системы и устранять неполадки, связанные с её запуском.  
