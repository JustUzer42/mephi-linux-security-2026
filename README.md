## Раздел 1. Создание пользователя

| Задача | Команда | Артефакт |
|---|---|---|
| Создание группы students | 'sudo groupadd students' | 'group' |
| Создание пользователя user1 с uid 1234 | 'sudo useradd -u 1234 -m -s /bin/bash -G students user1' | 'passwd', 'stat.out' |
| Установка смены пароля каждые 3 месяца | 'sudo chage -M 90 -W 7 user1' | 'shadow' |

Строки в артефакте history.out 

## Раздел 2. Мониторинг файлов и процессов

| Задача | Команда | Артефакт |
|---|---|---|
| Мониторинг файлов с set-UID | 'find / -type f -perm -4000 2>/dev/nu' | 'file_monitoring' |
| Мониторинг процессов EUID=0, RUID обычный | 'ps -eo ruid,euid,pid,comm | awk '$1 >= 1000 && $2 =='' | 'uid_monitoring.out' |

Строки в артефакте history.out 

## Раздел 3. Механизм set-UID
| Задача | Команда | Артефакт |
|---|---|---|
| Выбор и копирование утилиты cat в папку пользователя | 'cp /usr/bin/cat /home/alina/cat' | 'stat.out' |
| Назначение root владельцем файла  | 'chown root:root /home/alina/cat' | 'stat.out' |
| Установка set-UID  | '9  chmod u+s /home/alina/c' | 'stat.out' |

В итоге пользователь может прочитать '/etc/shadow' с помощью команды '~/cat /etc/shadow'. При проверке прав, установленных на скопированной утилите, выводятся права '-rwsr-xr-x (4755) root root'. Проверка выполнялась от пользователя командой 'ls -la ~cat'
Строки в артефакте history.out 

## Раздел 4. Механизм привилегий (capabilities)
| Задача | Команда | Артефакт |
|---|---|---|
| Выбор и копирование утилиты chown в папку пользователя | 'cp /bin/chown /home/alina/my_chown' | 'stat.out' |
| Установка capability на утилиту | 'setcap cap_chown=ep /home/alina/my_chown' | 'stat.out' |
| Проверка установленного capability | 'getcap /home/alina/my_chown' | 'stat.out', 'getcap.out' |

В итоге пользователь может изменить права доступа на созданный файл с помощью команды '/home/alina/my_chown root /home/aline/mytext.txt'. При проверке командой 'ls -la /home/alina/mytext.txt' вывелось '-rw-r--r--. 1 root alina'.
Строки в артефакте history.out 

## Раздел 5. Механизм sudo
| Задача | Команда | Артефакт |
|---|---|---|
| Настройка правила sudoers | 'visudo' | 'sudoers.out' |
Правило в '/etc/sudoers':

user1 ALL=(ALL) /usr/bin/timedatectl, /usr/bin/date 

В результате пользователь может сменить системное время.

## Артефакты

| Файл | Раздел |
|---|---|
| 'mephi_screenshot.png' | 6 |
| 'history.out' | все |
| 'stat.out' | 1, 3, 4 |
| 'getcap.out' | 4 |
| 'passwd', 'group' | 1 |
| 'sudoers' | 5 |
| 'file_monitoring' | 2.1 |
| 'uid_monitoring.out' | 2.2 |

Все настройки сохраняются в файлах на диске и переживают перезагрузку.
