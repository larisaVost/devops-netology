### Как сдавать задания

Вы уже изучили блок «Системы управления версиями», и начиная с этого занятия все ваши работы будут приниматься ссылками на .md-файлы, размещённые в вашем публичном репозитории.

Скопируйте в свой .md-файл содержимое этого файла; исходники можно посмотреть [здесь](https://raw.githubusercontent.com/netology-code/sysadm-homeworks/devsys10/04-script-02-py/README.md). Заполните недостающие части документа решением задач (заменяйте `???`, ОСТАЛЬНОЕ В ШАБЛОНЕ НЕ ТРОГАЙТЕ чтобы не сломать форматирование текста, подсветку синтаксиса и прочее, иначе можно отправиться на доработку) и отправляйте на проверку. Вместо логов можно вставить скриншоты по желани.

# Домашнее задание к занятию "4.2. Использование Python для решения типовых DevOps задач"

## Обязательная задача 1

Есть скрипт:
```python
#!/usr/bin/env python3
a = 1
b = '2'
c = a + b
```

### Вопросы:
| Вопрос  | Ответ |
| ------------- | ------------- |
| Какое значение будет присвоено переменной `c`?  | TypeError: unsupported operand type(s) for +: 'int' and 'str'  |
| Как получить для переменной `c` значение 12?  | str(a)+b  |
| Как получить для переменной `c` значение 3?  | a+int(b)  |

## Обязательная задача 2
Мы устроились на работу в компанию, где раньше уже был DevOps Engineer. Он написал скрипт, позволяющий узнать, какие файлы модифицированы в репозитории, относительно локальных изменений. Этим скриптом недовольно начальство, потому что в его выводе есть не все изменённые файлы, а также непонятен полный путь к директории, где они находятся. Как можно доработать скрипт ниже, чтобы он исполнял требования вашего руководителя?

```python
#!/usr/bin/env python3

import os

bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
        break
```

### Ваш скрипт:
```python
import os

dir_path = "C:\\Users\\LVost\\PycharmProjects\\devops-netology"
bash_command = [f"cd {dir_path}", "git status "]
result_os = os.popen(' && '.join(bash_command)).read()
#is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:', dir_path)
        print(prepare_result)
       # break

```

### Вывод скрипта при запуске при тестировании:
```
C:\Users\LVost\PycharmProjects\devops-netology   dop1.txt
C:\Users\LVost\PycharmProjects\devops-netology   dop5.txt

Process finished with exit code 0
```

## Обязательная задача 3
1. Доработать скрипт выше так, чтобы он мог проверять не только локальный репозиторий в текущей директории, а также умел воспринимать путь к репозиторию, который мы передаём как входной параметр. Мы точно знаем, что начальство коварное и будет проверять работу этого скрипта в директориях, которые не являются локальными репозиториями.

### Ваш скрипт:
```python
import os

dir_path = ""
try:    
    dir_path = input('Enter path to the local repository: ')
except:
    print("The path to the local repository is not specified")


if dir_path != "":
        try:
            bash_command = [f"cd {dir_path}",  "git status "]
            files = os.listdir(dir_path);
            if files.__contains__(".git"):
                    result_os = os.popen(' && '.join(bash_command)).read()
                    for result in result_os.split('\n'):
                        if result.find('modified') != -1:
                            prepare_result = result.replace('modified:', dir_path)
                            print(prepare_result)
            else:
                    print("There is no .git file")
        except FileNotFoundError:
            print("Wrong file or file path")

```

### Вывод скрипта при запуске при тестировании:
```
Enter path to the local repository: fghf
Wrong file or file path

или

Enter path to the local repository: C:\Users\LVost\PycharmProjects\devops-netology
	C:\Users\LVost\PycharmProjects\devops-netology   dop1.txt
	C:\Users\LVost\PycharmProjects\devops-netology   dop5.txt
```

## Обязательная задача 4
1. Наша команда разрабатывает несколько веб-сервисов, доступных по http. Мы точно знаем, что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, где установлен сервис. Проблема в том, что отдел, занимающийся нашей инфраструктурой очень часто меняет нам сервера, поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой DNS имена. Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой сегмент сети нашей компании, который недоступен для разработчиков. Мы хотим написать скрипт, который опрашивает веб-сервисы, получает их IP, выводит информацию в стандартный вывод в виде: <URL сервиса> - <его IP>. Также, должна быть реализована возможность проверки текущего IP сервиса c его IP из предыдущей проверки. Если проверка будет провалена - оповестить об этом в стандартный вывод сообщением: [ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. Будем считать, что наша разработка реализовала сервисы: `drive.google.com`, `mail.google.com`, `google.com`.

### Ваш скрипт:
```python
import socket
from string import whitespace
import datetime as dt

array_hosts = ["drive.google.com", "mail.google.com", "google.com"]
f_list = []

with open('check.log') as file:
    for j in file:
        f_list.append(j)

with open('check.log', 'w+') as file:
    for i in array_hosts:
        result = socket.gethostbyname(i)
        added = 0
        for y in f_list:
            in_list = y.find(" {}".format(i))
            if (in_list != -1):
                str_ip=y.replace('\n', '').split("  ")[1].translate({None: whitespace})
                if (str_ip == result):
                    print(str(dt.datetime.now().strftime("%Y-%m-%d %H:%M:%S"))," {}  {}\n".format(i, result))
                    file.write("{} {}  {}\n".format(str(dt.datetime.now().strftime("%Y-%m-%d %H:%M:%S")), i, result))
                    added = 1
                    break
                else:
                    print(str(dt.datetime.now().strftime("%Y-%m-%d %H:%M:%S")),"[ERROR] {} IP mismatch: {}  {}\n".format(i, str_ip, result))
                    file.write("{} [ERROR] {} IP mismatch: {}  {}\n".format(str(dt.datetime.now().strftime("%Y-%m-%d %H:%M:%S")),i, str_ip, result))
                    added = 1
                    break
        if (added == 0):
            print(" {}  {}\n".format(i, result))
            file.write(" {}  {}\n".format(i, result))

```

### Вывод скрипта при запуске при тестировании:
```
2022-05-26 22:05:08  drive.google.com  74.125.131.194

2022-05-26 22:05:08  mail.google.com  74.125.205.18

2022-05-26 22:05:08  google.com  216.239.38.120

2022-05-26 22:05:53  drive.google.com  74.125.131.194

2022-05-26 22:05:53 [ERROR] mail.google.com IP mismatch: 74.125.205.18  74.125.131.19

2022-05-26 22:05:53  google.com  216.239.38.120


```

## Дополнительное задание (со звездочкой*) - необязательно к выполнению

Так получилось, что мы очень часто вносим правки в конфигурацию своей системы прямо на сервере. Но так как вся наша команда разработки держит файлы конфигурации в github и пользуется gitflow, то нам приходится каждый раз переносить архив с нашими изменениями с сервера на наш локальный компьютер, формировать новую ветку, коммитить в неё изменения, создавать pull request (PR) и только после выполнения Merge мы наконец можем официально подтвердить, что новая конфигурация применена. Мы хотим максимально автоматизировать всю цепочку действий. Для этого нам нужно написать скрипт, который будет в директории с локальным репозиторием обращаться по API к github, создавать PR для вливания текущей выбранной ветки в master с сообщением, которое мы вписываем в первый параметр при обращении к py-файлу (сообщение не может быть пустым). При желании, можно добавить к указанному функционалу создание новой ветки, commit и push в неё изменений конфигурации. С директорией локального репозитория можно делать всё, что угодно. Также, принимаем во внимание, что Merge Conflict у нас отсутствуют и их точно не будет при push, как в свою ветку, так и при слиянии в master. Важно получить конечный результат с созданным PR, в котором применяются наши изменения. 

### Ваш скрипт:
```python
???
```

### Вывод скрипта при запуске при тестировании:
```
???
```
