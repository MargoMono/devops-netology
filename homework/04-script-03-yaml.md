**Задание 1**

Мы выгрузили JSON, который получили через API запрос к нашему сервису:

```json
    { "info" : "Sample JSON output from our service\t",
        "elements" :[
            { "name" : "first",
            "type" : "server",
            "ip" : 7175 
            }
            { "name" : "second",
            "type" : "proxy",
            "ip : 71.78.22.43
            }
        ]
    }
```

Нужно найти и исправить все ошибки, которые допускает наш сервис


Ваш скрипт:

```json
{
  "info": "Sample JSON output from our service\\t",
  "elements": [
    {
      "name": "first",
      "type": "server",
      "ip": 7175
    },
    {
      "name": "second",
      "type": "proxy",
      "ip": "71.78.22.43"
    }
  ]
}
```

**Задание 2**

В прошлый рабочий день мы создавали скрипт, позволяющий опрашивать веб-сервисы и получать их IP. К
уже реализованному функционалу нам нужно добавить возможность записи JSON и YAML файлов, описывающих наши сервисы. 

Формат записи JSON по одному сервису: { "имя сервиса" : "его IP"}. 
Формат записи YAML по одному сервису: - имя сервиса: его IP. 

Если в момент исполнения скрипта меняется IP у сервиса - он должен так же поменяться в yml и json файле.

```python
import socket
import os
import json
import yaml

def create_file_if_not_exist(file):
    if not os.path.exists(file):
        os.mknod(file)


base_log_file = 'log.txt'
create_file_if_not_exist(base_log_file)

log_file_json = 'log.json'
create_file_if_not_exist(log_file_json)

log_file_yaml = 'log.yaml'
create_file_if_not_exist(log_file_yaml)

host_name_list = ['drive.google.com', 'mail.google.com', 'google.com']
host_dict = {}


def parse_old_log_file():
    log_list = []
    with open(base_log_file, 'r') as old_file:
        while (line := old_file.readline().rstrip()):
            log_list.append(line)
    return log_list


with open(base_log_file, "w") as new_file:
    for host_name in ['drive.google.com', 'mail.google.com', 'google.com']:
        new_host_ip = socket.gethostbyname(host_name)
        new_host_ip_list = []
        new_host_ip_list.append(new_host_ip)

        for log in parse_old_log_file():
            log_host_name = log.split(' - ')[0]
            log_host_ip = log.split(' - ')[1]
            log_host_name_from_error = log.split(' ')[1]
            if log.find('[ERROR]') != -1 and log_host_name_from_error == host_name:
                new_file.write(log + '\n')
            if log_host_name == host_name and new_host_ip != log_host_ip:
                new_file.write('[ERROR] ' + host_name + ' IP mismatch: ' + log_host_ip + ' - ' + new_host_ip + '\n')
                break

        new_file.write(host_name + ' - ' + new_host_ip + '\n')
        host_dict[host_name] = new_host_ip_list

with open(log_file_json, "w") as log_file_json:
    json.dump(host_dict, log_file_json, indent=2)

with open(log_file_yaml, "w") as log_file_yaml:
    yaml.dump(host_dict, log_file_yaml)
```

json-файл(ы), который(е) записал ваш скрипт:

```json
{
  "drive.google.com": [
    "172.217.16.142"
  ],
  "mail.google.com": [
    "142.250.185.69"
  ],
  "google.com": [
    "172.217.23.110"
  ]
}
```

yml-файл(ы), который(е) записал ваш скрипт:

```yaml
drive.google.com:
- 172.217.16.142
google.com:
- 172.217.23.110
mail.google.com:
- 142.250.185.69
```