# Домашнее задание к занятию 11.4. «Очереди RabbitMQ» - Неудахин Денис
---

### Задание 1. Установка RabbitMQ

Используя Vagrant или VirtualBox, создайте виртуальную машину и установите RabbitMQ.
Добавьте management plug-in и зайдите в веб-интерфейс.

*Итогом выполнения домашнего задания будет приложенный скриншот веб-интерфейса RabbitMQ.*

<img src = "img/1.PNG" width = 100%>

---

### Задание 2. Отправка и получение сообщений

Используя приложенные скрипты, проведите тестовую отправку и получение сообщения.
Для отправки сообщений необходимо запустить скрипт producer.py.

Для работы скриптов вам необходимо установить Python версии 3 и библиотеку Pika.
Также в скриптах нужно указать IP-адрес машины, на которой запущен RabbitMQ, заменив localhost на нужный IP.

```shell script
$ pip install pika
```

Зайдите в веб-интерфейс, найдите очередь под названием hello и сделайте скриншот.
После чего запустите второй скрипт consumer.py и сделайте скриншот результата выполнения скрипта

*В качестве решения домашнего задания приложите оба скриншота, сделанных на этапе выполнения.*

Для закрепления материала можете попробовать модифицировать скрипты, чтобы поменять название очереди и отправляемое сообщение.

<img src = "img/2-1.PNG" width = 100%>

<img src = "img/2-2.PNG" width = 100%>
---

### Задание 3. Подготовка HA кластера

Используя Vagrant или VirtualBox, создайте вторую виртуальную машину и установите RabbitMQ.
Добавьте в файл hosts название и IP-адрес каждой машины, чтобы машины могли видеть друг друга по имени.

Пример содержимого hosts файла:
```shell script
$ cat /etc/hosts
192.168.0.10 rmq01
192.168.0.11 rmq02
```
После этого ваши машины могут пинговаться по имени.

Затем объедините две машины в кластер и создайте политику ha-all на все очереди.

*В качестве решения домашнего задания приложите скриншоты из веб-интерфейса с информацией о доступных нодах в кластере и включённой политикой.*

Также приложите вывод команды с двух нод:

```shell script
$ rabbitmqctl cluster_status
```

Для закрепления материала снова запустите скрипт producer.py и приложите скриншот выполнения команды на каждой из нод:

```shell script
$ rabbitmqadmin get queue='hello'
```

После чего попробуйте отключить одну из нод, желательно ту, к которой подключались из скрипта, затем поправьте параметры подключения в скрипте consumer.py на вторую ноду и запустите его.

*Приложите скриншот результата работы второго скрипта.*

Не получается создать кластер:
Сообщение на slave ноде.
```
attempted to contact: [rabbit@app1]

rabbit@app1:
  * connected to epmd (port 4369) on app1
  * epmd reports node 'rabbit' uses port 25672 for inter-node and CLI tool traffic
  * TCP connection succeeded but Erlang distribution failed
  * suggestion: check if the Erlang cookie identical for all server nodes and CLI tools
  * suggestion: check if all server nodes and CLI tools use consistent hostnames when addressing each other
  * suggestion: check if inter-node connections may be configured to use TLS. If so, all nodes and CLI tools must do that
   * suggestion: see the CLI, clustering and networking guides on https://rabbitmq.com/documentation.html to learn more


Current node details:
 * node name: 'rabbitmqcli-1257-rabbit@debstdclean2'
 * effective user's home directory: /var/lib/rabbitmq
 * Erlang cookie hash: YLuxUVo/+EN9FY5s6PXKOA==

```

файл .erlang.cookie
<img src = "img/3.PNG" width = 100%>
файл hosts
<img src = "img/4.PNG" width = 100%>

В документации написано, что нужно так же положить файл куки в домашнюю директорию что рута что юзера, что я и сделал но это ничего не изменило.
