# `Домашнее задание к занятию "Работа с данными (DDL/DML)"` - `Мешочков Александр SYS-22`



1. [Описание домашнего задания к занятию «Работа с данными (DDL/DML)»](https://github.com/netology-code/sdb-homeworks/blob/main/12-02.md)

---

 ### Задание 1: 
<details>
   <summary> Задание 1: </summary>

1.1. Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.

1.2. Создайте учётную запись sys_temp. 

1.3. Выполните запрос на получение списка пользователей в базе данных. (скриншот)

1.4. Дайте все права для пользователя sys_temp. 

1.5. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

1.6. Переподключитесь к базе данных от имени sys_temp.

Для смены типа аутентификации с sha2 используйте запрос: 
```sql
ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
1.6. По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.

1.7. Восстановите дамп в базу данных.

1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)

*Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.*
</details>

### Ответ 1
<details>
  <summary>Ответ 1</summary>
1.1. Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.

1.2. Создайте учётную запись sys_temp. 

1.3. Выполните запрос на получение списка пользователей в базе данных. (скриншот)

#### ОТВЕТ:
<img src = "image/git/12/DDL-DML.1.jpg" width = 100%>

1.4. Дайте все права для пользователя sys_temp. 

1.5. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

#### ОТВЕТ:
<img src = "image/git/12/DDL-DML.2.jpg" width = 100%)>

1.6. Переподключитесь к базе данных от имени sys_temp.

Для смены типа аутентификации с sha2 используйте запрос: 
```sql
ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
#### ОТВЕТ:
<img src = "image/git/12/DDL-DML.3.jpg" width = 100%>

1.6. По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.

1.7. Восстановите дамп в базу данных.

1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)

#### ОТВЕТ:
<img src = "image/git/12/DDL-DML.4.jpg" width = 100%>

*Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.*

#### ОТВЕТ (простыня со всеми запросами):
```bash
# ssh user@158.160.19.198 -i id_rsa
$ sudo -i
# apt update
# apt install mysql-server mysql-client
# mysqladmin password -u root -p
# mysql_secure_installation
# mysql -u root -p
mysql> CREATE USER 'sys_test'@'localhost' IDENTIFIED BY 'password';
mysql> SELECT user,authentication_string,plugin,host FROM mysql.user;
mysql> GRANT ALL PRIVILEGES ON *.* TO 'sys_test'@'localhost';
mysql> show grants for 'sys_test'@'localhost';
mysql> exit
# mysql -u sys_test -p
mysql> SELECT user();
mysql> exit
# wget https://downloads.mysql.com/docs/sakila-db.zip
# apt install unzip
# unzip sakila-db.zip
# mysql -u sys_test -p
mysql> CREATE DATABASE `sakila`;
mysql> SHOW DATABASES;
mysql> exit
# export DBNAME=sakila
# mysql -u sys_test -p ${DBNAME} < /root/sakila-db/sakila-schema.sql
# mysql -u sys_test -p ${DBNAME} < /root/sakila-db/sakila-data.sql
# mysql -u sys_test -p
mysql> SHOW DATABASES;
mysql> USE sakila;
mysql> SHOW TABLES;
```
</details>


--------

 ### Задание 2: 
<details>
   <summary> Задание 2: </summary>
  
Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца: в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц. Пример: (скриншот/текст)
```
Название таблицы | Название первичного ключа
customer         | customer_id
```
</details>

 ### Ответ 2: 
<details>
   <summary> Ответ 2: </summary>
#### ОТВЕТ:
  
<img src = "image/git/12/DDL-DML.5.jpg" width = 100%>
```
Название таблицы             | Название первичного ключа
actor                        | actor_id
actor_info                   | 
address                      | address_id
category                     | category_id
city                         | city_id
country                      | country_id
customer                     | customer_id
customer_list                | 
film                         | film_id
film_actor                   | actor_id, film_id
film_category                | film_id, category_id
film_list                    | 
film_text                    | film_id
inventory                    | inventory_id
language                     | language_id
nicer_but_slower_film_list   | 
payment                      | payment_id
rental                       | rental_id
sales_by_film_category       | 
sales_by_store               | 
staff                        | staff_id
staff_list                   | 
store                        | store_id
```
</details>

--------