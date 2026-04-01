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

Рис. 1 - Відкриття редактора crontab

Запис задачі у Cron:
```bash
*/1 * * * * echo "Hello Cron" >> /home/yaroslava/test.txt
```

<img width="826" height="559" alt="image" src="https://github.com/user-attachments/assets/1c898bb9-db74-46a6-980d-530f4bbe1dbd" />

Рис. 2 - Додавання задачі в Cron

Перегляд списку задач:
```bash
crontab -l
```

<img width="735" height="573" alt="image" src="https://github.com/user-attachments/assets/505972e4-ec16-4bbd-ad88-0d831d12ab23" />

Рис. 3 - Перегляд активних задач у Cron

Перевірка результату виконання:
```bash
cat /home/yaroslava/test.txt
```

<img width="598" height="166" alt="image" src="https://github.com/user-attachments/assets/a1aecb0c-c253-494b-8ffa-b1c3464b7baf" />

Рис. 4 - Вміст файлу після виконання задачі

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

Рис. 5 - Перелік активних таймерів systemd

- at

Одноразове виконання задачі:
```bash
at now + 1 minute
echo "AT task" >> /home/yaroslava/at.txt
<Ctrl+D>
```

<img width="517" height="157" alt="image" src="https://github.com/user-attachments/assets/33291780-b461-44c6-89d4-3ab099cfa3a4" />

Рис. 6 - Одноразова задача через at

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

Рис. 7 - Конфігурація Anacron

 ## Пункт 2: Планування задач у Linux через Cron

### 2.1 Виконання задачі в чітко визначений час

**Задача: запуск команди о 12:10**

Додаємо у crontab:
```bash
10 12 * * * echo "Task executed at 12:10" >> /home/yaroslava/scheduled_task.txt
```
<img width="938" height="779" alt="image" src="https://github.com/user-attachments/assets/611bd17f-cdc5-4616-8746-0845ebec1ad9" />

Перевірка crontab:
```bash
crontab -l
```

<img width="864" height="623" alt="image" src="https://github.com/user-attachments/assets/80669874-dd61-449d-9e9f-32188b693da4" />

Результат виконання:

<img width="731" height="255" alt="image" src="https://github.com/user-attachments/assets/a8453059-c0c2-4c5e-8d23-d039e358a327" />

✅ Задача виконана у точно спланований час.

### 2.2 Виконання однієї й тієї ж задачі двічі на день

**Задача: запуск команди о 12:15 та 12:20**

Cron-запис:
```bash
crontab -e
15,25 12 * * * echo "Task executed twice daily" >> /home/yaroslava/twice_daily_task.txt
```
<img width="944" height="617" alt="image" src="https://github.com/user-attachments/assets/3f2c6141-28b4-45c6-ab66-5bfbc009f07e" />

Перевірка crontab:
```bash
crontab -l
```
<img width="924" height="552" alt="image" src="https://github.com/user-attachments/assets/a511f6f5-d61d-40f2-9f19-4d39afcc47ea" />

Результат виконання:

<img width="702" height="110" alt="image" src="https://github.com/user-attachments/assets/bed7259d-299f-428b-ba6e-ada51fdc0ed2" />

✅ Задача виконана двічі на день.

### 2.3 Виконання задачі тільки в будні

**Задача: запуск команди о 12:30, 12:35, 12:40 у будні**

Cron-запис:
```bash
crontab -e
30,35,40 12 * * 1-5 echo "Weekday task executed" >> /home/yaroslava/weekday_task.txt
```

<img width="944" height="769" alt="image" src="https://github.com/user-attachments/assets/e562c315-e689-43bf-a924-2ab949509a1d" />

Перевірка crontab:
```bash
crontab -l
```

<img width="888" height="556" alt="image" src="https://github.com/user-attachments/assets/ac22ee69-8565-4bd2-8231-759c060a27f6" />

Результат виконання:

<img width="714" height="136" alt="image" src="https://github.com/user-attachments/assets/8110c9b5-6ec5-47d9-878f-e7224e56a90b" />

✅ Задача виконана лише в будні у визначений проміжок часу.

### 2.4 Виконання задач раз на рік, раз на місяць, раз на день, щогодини та після перезавантаження (демо)

Для демонстрації всі задачі виконуються кожну хвилину, щоб відразу бачити результат.

Cron-записи:
```bash
crontab -e
# Раз на рік (демо)
* * * * * echo "Yearly task executed (demo)" >> /home/yaroslava/demo_task.txt

# Раз на місяць (демо)
* * * * * echo "Monthly task executed (demo)" >> /home/yaroslava/demo_task.txt

# Раз на день (демо)
* * * * * echo "Daily task executed (demo)" >> /home/yaroslava/demo_task.txt

# Щогодини (демо)
* * * * * echo "Hourly task executed (demo)" >> /home/yaroslava/demo_task.txt

# Після перезавантаження (демо)
@reboot echo "Reboot task executed (demo)" >> /home/yaroslava/demo_task.txt

# Конкретний час для перевірки
0 15 * * * echo "Demo task executed at 15:00" >> /home/yaroslava/demo_task.txt
```
<img width="947" height="687" alt="image" src="https://github.com/user-attachments/assets/270cd77e-f079-4fef-9f00-30fa4ef65725" />

Перевірка crontab:
```bash
crontab -l
```

<img width="936" height="818" alt="image" src="https://github.com/user-attachments/assets/f75fb32a-cc8a-4472-84af-f2975688e04b" />

Результат виконання:

<img width="735" height="402" alt="image" src="https://github.com/user-attachments/assets/56dd46e2-a053-49b0-911b-b03a62e82555" />

<img width="312" height="150" alt="image" src="https://github.com/user-attachments/assets/cc424c66-7443-4ac3-9bb0-06a7f3120bc5" />

✅ Всі види періодичності задач успішно виконані.

2.5 Висновок
- Усі задачі додані до планувальника Cron.
- Файли створюються автоматично та містять очікувані результати.
- Виконання задач відповідає запланованому часу та дням.
- Демонстраційні задачі дозволили перевірити річні, місячні, щоденні та годинні завдання одразу.
