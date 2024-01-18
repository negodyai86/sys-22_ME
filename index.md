# `Домашнее задание к занятию "Индексы"` - `Мешочков Александр SYS-22`



1. [Описание домашнего задания к занятию «Индексы»](https://github.com/netology-code/sdb-homeworks/blob/main/12-05.md)
---

---
<details>
   <summary> Задание 1: </summary>
  
Напишите запрос к учебной базе данных, который вернёт процентное отношение общего размера всех индексов к общему размеру всех таблиц.

</details>

<details>
   <summary> Ответ 1: </summary>
  
```sql
SELECT SUM(index_length) / SUM(data_length) * 100
FROM INFORMATION_SCHEMA.TABLES;
```

</details>

---

---

<details>
   <summary> Задание 2: </summary>
  
Выполните explain analyze следующего запроса:
```sql
select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
```
- перечислите узкие места;
- оптимизируйте запрос: внесите корректировки по использованию операторов, при необходимости добавьте индексы.
  
</details>

<details>
   <summary> Ответ 2: </summary>
  
```sql
EXPLAIN ANALYZE
select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
```
```sql
-> Limit: 200 row(s)  (cost=0..0 rows=0) (actual time=8394..8394 rows=200 loops=1)
    -> Table scan on <temporary>  (cost=2.5..2.5 rows=0) (actual time=8394..8394 rows=200 loops=1)
        -> Temporary table with deduplication  (cost=0..0 rows=0) (actual time=8394..8394 rows=391 loops=1)
            -> Window aggregate with buffering: sum(payment.amount) OVER (PARTITION BY c.customer_id,f.title )   (actual time=3500..8028 rows=642000 loops=1) -- узкое место!
                -> Sort: c.customer_id, f.title  (actual time=3500..3625 rows=642000 loops=1) -- узкое место!
                    -> Stream results  (cost=21.2e+6 rows=16e+6) (actual time=6.7..2683 rows=642000 loops=1)
                        -> Nested loop inner join  (cost=21.2e+6 rows=16e+6) (actual time=6.5..2316 rows=642000 loops=1)
                            -> Nested loop inner join  (cost=19.6e+6 rows=16e+6) (actual time=3.71..2097 rows=642000 loops=1)
                                -> Nested loop inner join  (cost=18e+6 rows=16e+6) (actual time=3.69..1841 rows=642000 loops=1)
                                    -> Inner hash join (no condition)  (cost=1.58e+6 rows=15.8e+6) (actual time=0.608..117 rows=634000 loops=1)
                                        -> Filter: (cast(p.payment_date as date) = '2005-07-30')  (cost=1.65 rows=15813) (actual time=0.215..9.46 rows=634 loops=1)
                                            -> Table scan on p  (cost=1.65 rows=15813) (actual time=0.0336..6.16 rows=16044 loops=1)
                                        -> Hash
                                            -> Covering index scan on f using idx_title  (cost=103 rows=1000) (actual time=0.0449..0.278 rows=1000 loops=1)
                                    -> Covering index lookup on r using rental_date (rental_date=p.payment_date)  (cost=0.938 rows=1.01) (actual time=0.00166..0.0025 rows=1.01 loops=634000)
                                -> Single-row index lookup on c using PRIMARY (customer_id=r.customer_id)  (cost=250e-6 rows=1) (actual time=195e-6..228e-6 rows=1 loops=642000)
                            -> Single-row covering index lookup on i using PRIMARY (inventory_id=r.inventory_id)  (cost=925e-6 rows=1) (actual time=149e-6..179e-6 rows=1 loops=642000)
```
При анализе выполнения запроса были выявлены узкие места, проявляющиеся в значительном времени выполнения и обработке большого количества строк данных, достигающего 642 тысяч.
Основные проблемы возникали при сортировке данных по customer_id и f.title, а также при применении оконной агрегации и временных таблиц.

```sql
SELECT concat(c.last_name, ' ', c.first_name) AS customers, SUM(p.amount)
FROM customer c
JOIN rental r ON c.customer_id = r.customer_id 
JOIN payment p ON r.rental_date = p.payment_date WHERE date(p.payment_date) = '2005-07-30'
GROUP BY c.customer_id
```
```sql
EXPLAIN ANALYZE
SELECT concat(c.last_name, ' ', c.first_name) AS customers, SUM(p.amount)
FROM customer c
JOIN rental r ON c.customer_id = r.customer_id 
JOIN payment p ON r.rental_date = p.payment_date WHERE date(p.payment_date) = '2005-07-30'
GROUP BY c.customer_id;
```
```sql
-> Limit: 200 row(s)  (actual time=13.7..13.7 rows=200 loops=1)
    -> Table scan on <temporary>  (actual time=11.5..11.5 rows=200 loops=1)
        -> Aggregate using temporary table  (actual time=11.5..11.5 rows=391 loops=1)
            -> Nested loop inner join  (cost=12763 rows=16010) (actual time=0.119..10.3 rows=642 loops=1)
                -> Nested loop inner join  (cost=7160 rows=16010) (actual time=0.0976..9.29 rows=642 loops=1)
                    -> Filter: (cast(p.payment_date as date) = '2005-07-30')  (cost=1606 rows=15813) (actual time=0.064..7.08 rows=634 loops=1)
                        -> Table scan on p  (cost=1606 rows=15813) (actual time=0.044..5.51 rows=16044 loops=1)
                    -> Covering index lookup on r using rental_date (rental_date=p.payment_date)  (cost=0.25 rows=1.01) (actual time=0.0023..0.00324 rows=1.01 loops=634)
                -> Single-row index lookup on c using PRIMARY (customer_id=r.customer_id)  (cost=0.25 rows=1) (actual time=0.00125..0.00128 rows=1 loops=642)
```
Ввводим индекс:
```sql
CREATE INDEX payday ON payment(payment_date);
```
Результаты предыдущего EXPLAIN ANALYZE но с индексом:
```sql
-> Limit: 200 row(s)  (cost=8062 rows=187) (actual time=0.464..38.9 rows=200 loops=1)
    -> Group aggregate: sum(p.amount)  (cost=8062 rows=187) (actual time=0.463..38.8 rows=200 loops=1)
        -> Nested loop inner join  (cost=8043 rows=187) (actual time=0.3..38.4 rows=317 loops=1)
            -> Nested loop inner join  (cost=4022 rows=187) (actual time=0.096..14.6 rows=7694 loops=1)
                -> Index scan on c using PRIMARY  (cost=0.0228 rows=7) (actual time=0.0295..0.252 rows=284 loops=1)
                -> Index lookup on r using idx_fk_customer_id (customer_id=c.customer_id)  (cost=6.69 rows=26.7) (actual time=0.0395..0.0484 rows=27.1 loops=284)
            -> Index lookup on p using payday (payment_date=r.rental_date), with index condition: (cast(p.payment_date as date) = '2005-07-30')  (cost=0.25 rows=1) (actual time=0.00287..0.0029 rows=0.0412 loops=7694)
```

Для оптимизации запроса был внесен индекс на столбец payment_date в таблице payment, что значительно улучшило производительность запроса. 
После внесения изменений запрос стал значительно быстрее, сократив время выполнения с нескольких секунд до миллисекунд. Теперь запрос работает более эффективно и оптимизированно.

</details>

---