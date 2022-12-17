**Задание 1**

Есть скрипт:

```bash
a=1
b=2
c=a+b
d=$a+$b
e=$(($a+$b))
```

Какие значения переменным c,d,e будут присвоены? Почему?

```bash
a=1
b=2
c=a+b
d=$a+$b
e=$(($a+$b))
echo $c
echo $d
echo $e
```


| Переменная | Значение | Обоснование                                                                               |
|------------|----------|-------------------------------------------------------------------------------------------|
| c          | a+b      | При сложении a+b мы обращаемся не ранее объявленным переменным, а к строке 'a+b'          |
| d          | 1+2      | За счет символа $ произошло обращение к переменным, но по умолчанию они являются строками |
| e          | 3        | За счет скобок bash понимает, что необходимо произвести математическую операцию           |


**Задание 2**

На нашем локальном сервере упал сервис и мы написали скрипт, который постоянно проверяет его доступность, 
записывая дату проверок до тех пор, пока сервис не станет доступным (после чего скрипт должен завершиться). 
В скрипте допущена ошибка, из-за которой выполнение не может завершиться, при этом место на Жёстком Д
иске постоянно уменьшается. 

Что необходимо сделать, чтобы его исправить:

```bash
while ((1==1)
do
	curl https://localhost:4757
	if (($? != 0))
	then
		date >> curl.log
	fi
done
```

Ваш скрипт: "$ip":80

```bash 
while (( 1 == 1 ))
    do
        curl https://localhost:4757
        if (($? != 0))
        then
            date >> curl.log
            sleep 10
        else exit
        fi
    done
```

**Задание 3**

Необходимо написать скрипт, который проверяет доступность трёх IP о 80 порту и 
записывает результат в файл log. 
Проверять доступность необходимо пять раз для каждого узла.

IP:
- 192.168.0.1, 
- 173.194.222.113, 
- 87.250.250.242

Ваш скрипт:

```bash
IpList=(192.168.0.1 173.194.222.113 87.250.250.242)
for ip in "${IpList[@]}"
do
  attempt=1
  maxAttemptCount=5
  while((attempt<=maxAttemptCount))
    do
      ((attempt++))
      status=$(curl --connect-timeout 2 --write-out '%{http_code}' --silent --output /dev/null "$ip":80)
        if ((status==200)); then
            echo "Service $ip:80 is available" >>log.txt
        elif((status==000)); then
            echo "Service $ip:80 is unavailable" >>log.txt
        else
           echo "Service $ip:80 is available, but response code is $status" >>log.txt
        fi
    done
done
```

Файл
```bash
Service 192.168.0.1:80 is unavailable
Service 192.168.0.1:80 is unavailable
Service 192.168.0.1:80 is unavailable
Service 192.168.0.1:80 is unavailable
Service 192.168.0.1:80 is unavailable
Service 173.194.222.113:80 is available, but response code is 301
Service 173.194.222.113:80 is available, but response code is 301
Service 173.194.222.113:80 is available, but response code is 301
Service 173.194.222.113:80 is available, but response code is 301
Service 173.194.222.113:80 is available, but response code is 301
Service 87.250.250.242:80 is available, but response code is 406
Service 87.250.250.242:80 is available, but response code is 406
Service 87.250.250.242:80 is available, but response code is 406
Service 87.250.250.242:80 is available, but response code is 406
Service 87.250.250.242:80 is available, but response code is 406
```

**Задание 4**

Необходимо дописать скрипт из предыдущего задания так, чтобы он выполнялся до тех пор, 
пока один из узлов не окажется недоступным. 
Если любой из узлов недоступен - IP этого узла пишется в файл error, скрипт прерывается.

Ваш скрипт:
```bash
IpList=(192.168.0.1 173.194.222.113 87.250.250.242)
for ip in "${IpList[@]}"
do
  attempt=1
  maxAttemptCount=5
  while((attempt<=maxAttemptCount))
    do
      ((attempt++))
      status=$(curl --connect-timeout 2 --write-out '%{http_code}' --silent --output /dev/null "$ip":80)
        if ((status==000)); then
            echo "Service $ip:80 is not available" >>log.txt
            exit
        fi
    done
done
```