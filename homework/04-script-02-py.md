**Задание 1**

Есть скрипт:

```bash
#!/usr/bin/env python3
a = 1
b = '2'
c = a + b
```

Вопросы:

| Вопрос                                            | Ответ                                                                                                                                              |                                                                                
|---------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| Какое значение будет присвоено переменной c       | Никакого, скрипт упадет в всязи с тем, что мы пытаемся сложить разные типы<br/>```TypeError: unsupported operand type(s) for +: 'int' and 'str'``` | 
| Как получить для переменной c значение 12?        | Привести а к тику sting, как вариант сделать так```c = str(a) + b```                                                                               |
| Как получить для переменной c значение 3          | Привести а к типу integer, как вариант сделать так```c = a + int(b)```                                                                                           | 

**Задание 2**

Мы устроились на работу в компанию, где раньше уже был DevOps Engineer. 
Он написал скрипт, позволяющий узнать, какие файлы модифицированы в репозитории, 
относительно локальных изменений. Этим скриптом недовольно начальство,
потому что в его выводе есть не все изменённые файлы, 
а также непонятен полный путь к директории, где они находятся.

Как можно доработать скрипт ниже, чтобы он исполнял требования вашего руководителя?

```bash
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

Ваш скрипт

```bash
#!/usr/bin/env python3
import os

import os

base_path = os.path.dirname(os.path.abspath(__file__))
bash_command = [f'cd {base_path}', "git status"]
result_os = os.popen(' && '.join(bash_command)).read()

for result in result_os.split('\n'):
    if result.find('изменено') != -1:
        print(base_path + '/' + result.replace('изменено:', '').strip())
```

Результат:

```bash
margo@margo-HP-Laptop-15s-eq0xxx:~/PycharmProjects/devops-netology/homework$ /usr/bin/env python3 /home/margo/PycharmProjects/devops-netology/homework/script.sh
/PycharmProjects/devops-netology/homework/04-script-02-py.md
/PycharmProjects/devops-netology/homework/script.sh
```

**Задание 3**
Доработать скрипт выше так, чтобы он не только мог проверять локальный репозиторий в текущей директории, 
но и умел воспринимать путь к репозиторию, который мы передаём как входной параметр. 
Мы точно знаем, что начальство коварное и будет проверять работу этого скрипта в директориях, 
которые не являются локальными репозиториями.

Ваш скрипт:
```bash
#!/usr/bin/env python3
import os
import sys

try:
    base_path = sys.argv[1]
except:
    print('Parameter "file path" was not passed, will check current repository')
    base_path = os.path.dirname(os.path.abspath(__file__))

if os.path.isdir(base_path) == False:
    print(f'Directory "{base_path}" not exist')
    sys.exit()

bash_command = [f'cd {base_path}', "git status"]
result_os = os.popen(' && '.join(bash_command)).read()

for result in result_os.split('\n'):
    if result.find('изменено') != -1:
        print(base_path + '/' + result.replace('изменено:', '').strip())
```

Результаты

```bash
margo@margo-HP-Laptop-15s-eq0xxx:~/PycharmProjects/devops-netology/homework$ /usr/bin/env python3 /home/margo/PycharmProjects/devops-netology/homework/script.sh
Parameter "file path" was not passed, will check current repository
/home/margo/PycharmProjects/devops-netology/homework/04-script-02-py.md
/home/margo/PycharmProjects/devops-netology/homework/script.sh
/home/margo/PycharmProjects/devops-netology/homework/script1.sh
margo@margo-HP-Laptop-15s-eq0xxx:~/PycharmProjects/devops-netology/homework$ /usr/bin/env python3 /home/margo/PycharmProjects/devops-netology/homework/script.sh test
Directory "test" not exist
margo@margo-HP-Laptop-15s-eq0xxx:~/PycharmProjects/devops-netology/homework$ /usr/bin/env python3 /home/margo/PycharmProjects/devops-netology/homework/script.sh /home/margo/PycharmProjects/devops-netology/homework
/home/margo/PycharmProjects/devops-netology/homework/04-script-02-py.md
/home/margo/PycharmProjects/devops-netology/homework/script.sh
/home/margo/PycharmProjects/devops-netology/homework/script1.sh
```

**Задание 4**

Наша команда разрабатывает несколько веб-сервисов, доступных по http. 
Мы точно знаем, что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, 
где установлен сервис.

Проблема в том, что отдел, занимающийся нашей инфраструктурой очень часто меняет нам сервера, 
поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой DNS имена. 
Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой сегмент сети нашей компании, 
который недоступен для разработчиков.

Мы хотим написать скрипт, который:
- опрашивает веб-сервисы,
- получает их IP,
- выводит информацию в стандартный вывод в виде: <URL сервиса> - <его IP>.

Также, должна быть реализована возможность проверки текущего IP сервиса c его IP из предыдущей проверки. 
Если проверка будет провалена - оповестить об этом в стандартный вывод сообщением:
[ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. 

Будем считать, что наша разработка реализовала сервисы: 
- drive.google.com, 
- mail.google.com, 
- google.com.

```python
import socket
import os

log_file = 'log.txt'

if not os.path.exists(log_file):
    os.mknod(log_file)

host_name_list = ['drive.google.com', 'mail.google.com', 'google.com']
log_list = []

with open(log_file, 'r') as old_file:
    while (line := old_file.readline().rstrip()):
        log_list.append(line)

with open(log_file, "w") as new_file:
    for host_name in host_name_list:
        new_host_ip = socket.gethostbyname(host_name)

        for log in log_list:
            log_host_name = log.split(' - ')[0]
            log_host_ip = log.split(' - ')[1]
            log_host_name_from_error = log.split(' ')[1]
            if log.find('[ERROR]') != -1 and log_host_name_from_error == host_name:
                new_file.write(log + '\n')
            if log_host_name == host_name and new_host_ip != log_host_ip:
                new_file.write('[ERROR] ' + host_name + ' IP mismatch: ' + log_host_ip + ' - ' + new_host_ip + '\n')
                break

        new_file.write(host_name + ' - ' + new_host_ip + '\n')

```

Пример вывода (рандомно меняла значения старых ip):

```text
[ERROR] drive.google.com IP mismatch: 172.217.16.147 - 172.217.16.142
[ERROR] drive.google.com IP mismatch: 172.217.16.149 - 172.217.16.142
drive.google.com - 172.217.16.142
[ERROR] mail.google.com IP mismatch: 142.250.185.66 - 142.250.185.69
[ERROR] mail.google.com IP mismatch: 142.250.185.67 - 142.250.185.69
mail.google.com - 142.250.185.69
google.com - 172.217.23.110
```