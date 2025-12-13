---
## Front matter
title: "Отчёт по лабораторной работе №15"
subtitle: "Управление логическими томами"
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

Получить навыки управления логическими томами.

# Ход выполнения

## Создание физического тома

В начале работы были отключены ранее созданные точки монтирования.  
В файле `/etc/fstab` строки монтирования **/mnt/data** и **/mnt/data-ext** были закомментированы, затем каталоги размонтированы командой `umount`.  
Проверка через `mount` показала, что устройства **/dev/sdb** и **/dev/sdc** не используются.

### Подготовка диска /dev/sdb

С помощью `fdisk` была создана новая таблица разделов.  
Просмотр текущей разметки выполнялся командой `p`, затем создана новая таблица DOS с помощью `o`.  
После этого изменения сохранены (`w`) и команда `partprobe` обновила таблицу разделов ядра.  
Информация о диске проверена через `cat /proc/partitions` и `fdisk --list /dev/sdb`.

Далее создан основной раздел размером **300 МБ** и изменён его тип на **8e (Linux LVM)**.  

![Создание нового раздела](Screenshot_1.png){ #fig:001 width=70% }

### Создание физического тома

После обновления таблицы разделов (`partprobe /dev/sdb`) раздел /dev/sdb1 был преобразован в физический том:

`pvcreate /dev/sdb1`

Проверка созданных физических томов:

`pvs`

![Создание PV](Screenshot_2.png){ #fig:002 width=70% }

## Создание группы томов и логического тома

Проверка доступных PV показала наличие нового физического тома.  
Создана группа томов:

`vgcreate vgdata /dev/sdb1`

Проверка через `vgs` и дополнительный просмотр `pvs` подтвердили успешное создание VG.

После этого создан логический том, занимающий 50% свободного места в группе:

`lvcreate -n lvdata -l 50%FREE vgdata`

Проверка:

`lvs`

![Создание VG и LV](Screenshot_3.png){ #fig:003 width=70% }

## Создание файловой системы и монтирование

Создана файловая система EXT4:

`mkfs.ext4 /dev/vgdata/lvdata`

Создан каталог для монтирования:

`mkdir -p /mnt/data`

В `/etc/fstab` добавлена строка:

`/dev/vgdata/lvdata /mnt/data ext4 defaults 1 2`

![fstab](Screenshot_4.png){ #fig:004 width=70% }

После выполнения `mount -a` файловая система успешно примонтирована.  
Проверка через `mount | grep mnt` подтвердила это.

![Проверка монтирования](Screenshot_5.png){ #fig:005 width=70% }

## Увеличение размера логического тома

Перед расширением были просмотрены текущие PV и VG:

`pvs`  
`vgs`

### Создание раздела /dev/sdb2

Через `fdisk` создан второй раздел размером **300 МБ** с типом **8e (Linux LVM)**.

![Создание второго раздела](Screenshot_6.png){ #fig:006 width=70% }

Создание физического тома:

`pvcreate /dev/sdb2`

Расширение группы томов:

`vgextend vgdata /dev/sdb2`

Проверка изменения размера группы:

`vgs`

![Расширение VG](Screenshot_7.png){ #fig:007 width=70% }

### Увеличение LV и файловой системы

Проверка текущих размеров:

`lvs`  
`df -h`

Наращивание на 50% от свободного места:

`lvextend -r -l +50%FREE /dev/vgdata/lvdata`

![Увеличение LV](Screenshot_8.png){ #fig:008 width=70% }

Повторная проверка:

`lvs`  
`df -h`

## Уменьшение логического тома

Размер логического тома уменьшен на 50 МБ:

`lvreduce -r -L -50M /dev/vgdata/lvdata`

В процессе том временно размонтировался, после чего был примонтирован обратно.

Финальная проверка:

`lvs`  
`df -h`

![Уменьшение LV](Screenshot_9.png){ #fig:009 width=70% }

# Самостоятельная работа

## Создание логического тома lvgroup

Для начала был подготовлен диск **/dev/sdc**, на котором создан новый раздел размером **400 МБ** и изменён его тип на **8e (Linux LVM)**.  
При наличии сигнатуры XFS на разделе она была удалена.  

![Создание раздела](Screenshot_10.png){ #fig:010 width=70% }

После записи таблицы разделов были выполнены:

- обновление таблицы ядра (`partprobe`)
- создание физического тома: **pvcreate /dev/sdc1**
- создание группы томов: **vgcreate vggroup /dev/sdc1**
- создание логического тома **lvgroup** размером 100% пространства VG

![Создание PV, VG и LV](Screenshot_11.png){ #fig:011 width=70% }

## Форматирование в XFS и монтирование

Создан каталог для монтирования:

`mkdir -p /mnt/groups`

Логический том отформатирован в XFS:

`mkfs.xfs /dev/vggroup/lvgroup`

![Форматирование XFS](Screenshot_12.png){ #fig:012 width=70% }

Добавлена строка в `/etc/fstab`:

![fstab](Screenshot_13.png){ #fig:013 width=70% }

Файловая система успешно подключена:

![Проверка монтирования](Screenshot_14.png){ #fig:014 width=70% }

## Увеличение размера логического тома на 150 МБ

Создан новый раздел **/dev/sda2** размером 300 МБ и установлен тип **8e**:

![Создание раздела sda2](Screenshot_15.png){ #fig:015 width=70% }

Раздел преобразован в физический том:

`pvcreate /dev/sda2`

VG расширена:

`vgextend vggroup /dev/sda2`

Далее логический том расширен на 100% доступного пространства:

`lvextend -r -l +100%FREE /dev/vggroup/lvgroup`

Файловая система XFS была увеличена автоматически с помощью xfs_growfs.

![Расширение LV и FS](Screenshot_16.png){ #fig:016 width=70% }

## Финальная проверка

Проверка PV, VG, LV:

![Проверка LV, VG, PV](Screenshot_17.png){ #fig:017 width=70% }

Проверка файловой системы:

`df -h`

Показано увеличение размера:

- LV стал **692 МБ**
- Файловая система XFS также расширилась до нового размера.

# Контрольные вопросы

**1. Какой тип раздела используется в разделе GUID для работы с LVM?**
Для LVM в GUID-разметке используется тип раздела **8e00 (Linux LVM)**.

**2. Какой командой можно создать группу томов с именем vggroup, которая содержит физическое устройство /dev/sdb3 и использует физический экстент 4 MiB?**
Для указания размера экстента используется опция `-s`:
`vgcreate -s 4M vggroup /dev/sdb3`

**3. Какая команда показывает краткую сводку физических томов в вашей системе, а также группу томов, к которой они принадлежат?**
Для краткого вывода физических томов используется команда:
`pvs`

**4. Что вам нужно сделать, чтобы добавить весь жёсткий диск /dev/sdd в группу томов группы?**
Нужно:

1. создать раздел LVM (тип 8e) на диске /dev/sdd
2. выполнить: `pvcreate /dev/sdd1`
3. добавить в VG: `vgextend <имя_группы> /dev/sdd1`

**5. Какая команда позволяет создать логический том lvvol1 с размером 6 MiB?**
Используется команда:
`lvcreate -n lvvol1 -L 6M <имя_группы>`

**6. Какая команда позволяет добавить 100 МБ в логический том lvvol1, если в группе томов достаточно свободного места?**
Команда увеличения тома:
`lvextend -L +100M /dev/<vgname>/lvvol1`


**7. Каков первый шаг, чтобы добавить ещё 200 МБ дискового пространства в логический том, если требуемого места нет в группе томов?**
Сначала нужно **расширить группу томов**, добавив в неё новый физический том:

1. создать новый PV: `pvcreate /dev/<новый_раздел>`
2. расширить VG: `vgextend <vgname> /dev/<новый_раздел>`

**8. Какую опцию нужно использовать с командой lvextend, чтобы также изменить размер файловой системы?**
Используется опция:
`-r`
Она позволяет расширить LV и автоматически увеличить файловую систему.

**9. Как посмотреть, какие логические тома доступны?**
Для этого применяется команда:
`lvs`
или более подробный вариант:
`lvdisplay`

**10. Какую команду нужно использовать для проверки целостности файловой системы на /dev/vgdata/lvdata?**
Это зависит от типа файловой системы:

* для ext4: `fsck.ext4 /dev/vgdata/lvdata`
* универсальный вариант: `fsck /dev/vgdata/lvdata`

# Заключение

В ходе работы был выполнен полный цикл управления логическими томами в Linux: от создания физических томов и групп томов до формирования логических разделов, их форматирования и постоянного монтирования. Были применены операции изменения размеров томов — как увеличения, так и последующего расширения файловой системы. Дополнительно изучены процессы расширения группы томов за счёт подключения новых физических устройств.

