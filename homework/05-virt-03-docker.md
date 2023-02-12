---------------------------------
Задача 1

---------------------------------

Сценарий выполения задачи:

    создайте свой репозиторий на https://hub.docker.com;
    выберете любой образ, который содержит веб-сервер Nginx;
    создайте свой fork образа;
    реализуйте функциональность: запуск веб-сервера в фоне с индекс-страницей, содержащей HTML-код ниже:

```html
<html>
    <head>Hey, Netology</head>
    <body>
        <h1>I’m DevOps Engineer!</h1>
    </body>
</html>
```

Опубликуйте созданный форк в своем репозитории и предоставьте ответ в виде ссылки на https://hub.docker.com/username_repo.

**ОТВЕТ**

https://hub.docker.com/repository/docker/margomono/margo_docker

---------------------------------
Задача 2
---------------------------------

Посмотрите на сценарий ниже и ответьте на вопрос:
"Подходит ли в этом сценарии использование Docker контейнеров или 
лучше подойдет виртуальная машина, физическая машина? 
Может быть возможны разные варианты?"

Детально опишите и обоснуйте свой выбор.

--

Сценарий:

    Высоконагруженное монолитное java веб-приложение;
    Nodejs веб-приложение;
    Мобильное приложение c версиями для Android и iOS;
    Шина данных на базе Apache Kafka;
    Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;
    Мониторинг-стек на базе Prometheus и Grafana;
    MongoDB, как основное хранилище данных для java-приложения;
    Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry.

**ОТВЕТ**

**Высоконагруженное монолитное java веб-приложение**
Для прод окруения данного монолита я бы выбрала физическую машину по причине высоких нагрузок и того, 
что приложение не трубует разноса по серверам

**Nodejs веб-приложение**
Docker вполне подойдет - простота в части деплоя и управления

**Мобильное приложение c версиями для Android и iOS**
Docker вполне подойдет - простота в части деплоя и управления

**Шина данных на базе Apache Kafka**
Docker вполне подойдет - простота в части масштабирования и управления

**Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana**
Docker вполне подойдет - простота в части масштабирования и управления

**Мониторинг-стек на базе Prometheus и Grafana**

Docker вполне подойдет - простота в части масштабирования и управления

**MongoDB, как основное хранилище данных для java-приложения**

Я бы вырала обланчые решения - сейчас они самые надежыные на рынке. Все решения, 
что предложены по задаче тут имеют приличное количество минусов. 

**Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry**

Отдельный физический сервер или виртуализация поскольку требует стабильной работы 
и не требуется дублировать в локальное окружение для разработчиков

---------------------------------
Задача 3
---------------------------------

    Запустите первый контейнер из образа centos c любым тэгом в фоновом режиме, подключив папку /data из текущей рабочей директории на хостовой машине в /data контейнера;
    Запустите второй контейнер из образа debian в фоновом режиме, подключив папку /data из текущей рабочей директории на хостовой машине в /data контейнера;
    Подключитесь к первому контейнеру с помощью docker exec и создайте текстовый файл любого содержания в /data;
    Добавьте еще один файл в папку /data на хостовой машине;
    Подключитесь во второй контейнер и отобразите листинг и содержание файлов в /data контейнера.


**ОТВЕТ**

Листинг кода

```bash
margo@margo-ASUS-TUF-Gaming-F17-FX707ZM-FX707ZM:~/Projects/devops-netology/practice/05-virt-03-docker/part-2$ docker run -d -v /data:/data centos sleep infinity
Unable to find image 'centos:latest' locally
latest: Pulling from library/centos
a1d0c7532777: Pull complete 
Digest: sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
Status: Downloaded newer image for centos:latest
45b635bf26b36b4492b8b2aa2e3111df8a9a4e106f89ce4c47150526b6cf493a
margo@margo-ASUS-TUF-Gaming-F17-FX707ZM-FX707ZM:~/Projects/devops-netology/practice/05-virt-03-docker/part-2$  docker ps
CONTAINER ID   IMAGE                              COMMAND                  CREATED          STATUS          PORTS                                                                                                                                                 NAMES
45b635bf26b3   centos                             "sleep infinity"         47 seconds ago   Up 46 seconds                                                                                                                                                         sweet_rubin
margo@margo-ASUS-TUF-Gaming-F17-FX707ZM-FX707ZM:~/Projects/devops-netology/practice/05-virt-03-docker/part-2$ exec -it 867d3a86aa2a bash^C
margo@margo-ASUS-TUF-Gaming-F17-FX707ZM-FX707ZM:~/Projects/devops-netology/practice/05-virt-03-docker/part-2$ docker exec -it 45b635bf26b3 bash
[root@45b635bf26b3 /]# echo 'Test' > /data/test-1.txt     
[root@45b635bf26b3 data]# exit
exit
margo@margo-ASUS-TUF-Gaming-F17-FX707ZM-FX707ZM:~/Projects/devops-netology/practice/05-virt-03-docker/part-2$ docker run -v /data:/data -d debian sleep infinity
Unable to find image 'debian:latest' locally
latest: Pulling from library/debian
1e4aec178e08: Pull complete 
Digest: sha256:43ef0c6c3585d5b406caa7a0f232ff5a19c1402aeb415f68bcd1cf9d10180af8
Status: Downloaded newer image for debian:latest
38822bc4790ebe7959e79109eff7376311f35c3f730b9c3a700876027130c61f
margo@margo-ASUS-TUF-Gaming-F17-FX707ZM-FX707ZM:~/Projects/devops-netology/practice/05-virt-03-docker/part-2$ docker ps
CONTAINER ID   IMAGE                              COMMAND                  CREATED              STATUS              PORTS                                                                                                                                                 NAMES
38822bc4790e   debian                             "sleep infinity"         About a minute ago   Up About a minute                                                                                                                                                         epic_elgamal
45b635bf26b3   centos                             "sleep infinity"         5 minutes ago        Up 5 minutes                                                                                                                                                              sweet_rubin
margo@margo-ASUS-TUF-Gaming-F17-FX707ZM-FX707ZM:~/Projects/devops-netology/practice/05-virt-03-docker/part-2$ docker exec -it docker run -v /data:/data -d debian sleep infinity^C
margo@margo-ASUS-TUF-Gaming-F17-FX707ZM-FX707ZM:~/Projects/devops-netology/practice/05-virt-03-docker/part-2$ docker exec -it 38822bc4790e bash
root@38822bc4790e:/# cat data/test-1.txt 
Test
root@38822bc4790e:/#
```