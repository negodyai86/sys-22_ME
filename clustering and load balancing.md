# `Домашнее задание к занятию "Кластеризация и балансировка нагрузки"` - `Мешочков Александр SYS-22`



1. [Описание домашнего задания к занятию «Кластеризация и балансировка нагрузки»](https://github.com/netology-code/sflt-homeworks/blob/main/2.md)

---

1. ### Задание 1

   - Запустите два simple python сервера на своей виртуальной машине на разных портах
   - Установите и настройте HAProxy, воспользуйтесь материалами к лекции по [ссылке](https://github.com/netology-code/sflt-homeworks/blob/main/2)
   - Настройте балансировку Round-robin на 4 уровне.
   - На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy.



### Ответ 1

Отредактировал конфигурационный файл HAProxy:

```bash
sudo nano /etc/haproxy/haproxy.cfg
```

```bash

frontend example  # секция фронтенд
        mode tcp
        bind :8088
        default_backend web_servers

backend web_servers    # секция бэкенд
        mode tcp
        balance roundrobin
        option httpchk
        http-check send meth GET uri /index.html
        server s1 127.0.0.1:8888 check
        server s2 127.0.0.1:9999 check
```

<img src = "image/git/11/11.1.jpg" width = 100%>







--------

1. ### Задание 2

   - Запустите три simple python сервера на своей виртуальной машине на разных портах
   - Настройте балансировку Weighted Round Robin на 7 уровне, чтобы первый сервер имел вес 2, второй - 3, а третий - 4
   - HAproxy должен балансировать только тот http-трафик, который адресован домену example.local
   - На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy c использованием домена example.local и без него.

### Ответ 2

Отредактировал конфигурационный файл HAProxy:

```bash
sudo nano /etc/haproxy/haproxy.cfg
```

```bash
frontend example  # секция фронтенд
        mode http
        bind :8088
	acl ACL_example.com hdr(host) -i example.local
	use_backend web_servers if ACL_example.com

backend web_servers    # секция бэкенд
        mode http
        balance roundrobin
        option httpchk
        http-check send meth GET uri /index.html
        server s1 127.0.0.1:8888 weight 2 check
        server s2 127.0.0.1:9999 weight 3 check
        server s3 127.0.0.1:7777 weight 4 check
```

<img src = "image/git/11/11.2.jpg" width = 100%>

<img src = "image/git/11/11.3.jpg" width = 100%>
