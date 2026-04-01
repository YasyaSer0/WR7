# МІНІСТЕРСТВО ОСВІТИ І НАУКИ УКРАЇНИ  
## КИЇВСЬКИЙ ФАХОВИЙ КОЛЕДЖ ЗВ’ЯЗКУ  

---

# ЗВІТ  
## про виконання work-case №7
### з дисципліни «Операційні системи»

---

**Виконала:**  
студентка групи **БІКС-33**  
**Сербіна Ярослава Вячеславівна**

---

**Перевірила:**  
**Сушанова Вікторія Сергіївна**

---

**Київ - 2026**

---

# Work-case №7
## Операційні системи

---

## Пункт 1: Планування задач
### 1.1 Основні функції планувальника задач

Планувальник задач - це системний механізм, який дозволяє автоматизувати виконання команд та скриптів у визначений час або за певних умов. Основні функції будь-якого планувальника задач:
- Автоматичний запуск процесів без участі користувача.
- Виконання задач у повторюваному режимі (щохвилини, щогодини, щодня, щотижня).
- Виконання задач при настанні певної події (вмикання системи, завантаження користувача).
- Логування результатів виконання та повідомлення про помилки.
- Управління пріоритетами та умовами виконання задач.

<img width="1180" height="831" alt="image" src="https://github.com/user-attachments/assets/96250e05-1576-4898-b48d-27a1cf446c1c" />

### 1.2 Порівняння планувальників у Windows та Linux
| Параметр                           | Windows (Task Scheduler)                         | Linux (Cron, systemd timers, at, anacron)    |
| ---------------------------------- | ------------------------------------------------ | -------------------------------------------- |
| Тип задач                          | Автоматичний запуск, запуск при події            | Автоматичний запуск, повторювані задачі      |
| Повторюваність                     | Гнучкі налаштування (щогодини, щодня, при вході) | Cron: точний розклад, systemd timers: гнучко |
| Інтерфейс                          | Графічний (GUI) + командний рядок                | Командний рядок (`crontab`, `systemctl`)     |
| Логування                          | Є стандартне логування                           | Є через syslog або systemd journal           |
| Альтернатива для одноразових задач | Task Scheduler + PowerShell скрипти              | `at`, `anacron`                              |

### 1.3 Планувальник Cron у Linux

Принцип роботи:

Cron - системний планувальник задач, який автоматично виконує команди у визначений час за допомогою файлу crontab.

Формат запису Cron-завдань:
```bash
m h dom mon dow command
````
Приклад задачі Cron:
```bash
*/1 * * * * echo "Hello Cron" >> /home/yaroslava/test.txt
```
→ виконується щохвилини та записує рядок у файл test.txt.

Налаштування:
```bash
crontab -e   # редагування задач
crontab -l   # перегляд задач
```
Перевірка результату виконання:
```bash
cat /home/yaroslava/test.txt
```
Результат:
```bash
Hello
World
Hello Cron
Hello Cron
```

<img width="785" height="502" alt="image" src="https://github.com/user-attachments/assets/bf3fd7bf-abc5-4b87-a6c7-27b300f9cb07" />

Рис. 1 – Відкриття редактора crontab

Запис задачі у Cron:
```bash
*/1 * * * * echo "Hello Cron" >> /home/yaroslava/test.txt
```

<img width="826" height="559" alt="image" src="https://github.com/user-attachments/assets/1c898bb9-db74-46a6-980d-530f4bbe1dbd" />

Рис. 2 – Додавання задачі в Cron

Перегляд списку задач:
```bash
crontab -l
```

<img width="735" height="573" alt="image" src="https://github.com/user-attachments/assets/505972e4-ec16-4bbd-ad88-0d831d12ab23" />

Рис. 3 – Перегляд активних задач у Cron

Перевірка результату виконання:
```bash
cat /home/yaroslava/test.txt
```

<img width="598" height="166" alt="image" src="https://github.com/user-attachments/assets/a1aecb0c-c253-494b-8ffa-b1c3464b7baf" />

Рис. 4 – Вміст файлу після виконання задачі

### 1.4 Альтернативи Cron

- systemd timers

Перегляд активних таймерів:
```bash
systemctl list-timers
```
Результат:

NEXT                            LEFT LAST                          PASSED UNIT
Wed 2026-04-01 11:08:29 UTC 1min 25s -                                  - systemd-tm>
Wed 2026-04-01 11:10:00 UTC 2min 56s Wed 2026-04-01 11:00:17 UTC 6min ago sysstat-co>
...

<img width="871" height="477" alt="image" src="https://github.com/user-attachments/assets/f07b55ea-133c-485b-b75e-0a68411f7d5c" />

Рис. 5 – Перелік активних таймерів systemd

- at

Одноразове виконання задачі:
```bash
at now + 1 minute
echo "AT task" >> /home/yaroslava/at.txt
<Ctrl+D>
```

<img width="517" height="157" alt="image" src="https://github.com/user-attachments/assets/33291780-b461-44c6-89d4-3ab099cfa3a4" />

Рис. 6 – Одноразова задача через at

- anacron

Виконує задачі, які могли бути пропущені (наприклад, якщо комп’ютер був вимкнений).

Перевірка конфігурації Anacron:
```bash
cat /etc/anacrontab
```

Вивід терміналу:

```bash
# /etc/anacrontab: configuration file for anacron

# See anacron(8) and anacrontab(5) for details.

SHELL=/bin/sh
HOME=/root
LOGNAME=root

# These replace cron's entries
1	5	cron.daily	run-parts --report /etc/cron.daily
7	10	cron.weekly	run-parts --report /etc/cron.weekly
@monthly	15	cron.monthly	run-parts --report /etc/cron.monthly
```

<img width="796" height="329" alt="image" src="https://github.com/user-attachments/assets/ddfca654-db8e-4723-bf11-b14c5e713ea1" />

Рис. 7 – Конфігурація Anacron
