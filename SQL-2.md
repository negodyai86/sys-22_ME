# `Домашнее задание к занятию "SQL. Часть 2"` - `Мешочков Александр SYS-22`



1. [Описание домашнего задания к занятию «SQL. Часть 2»](https://github.com/netology-code/sdb-homeworks/blob/main/12-04.md)
---

---
<details>
   <summary> Задание 1: </summary>
Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.
</details>

<details>
   <summary> Ответ 1: </summary>
  
```sql
SELECT CONCAT(s.last_name, ' ', s.first_name) AS staff, c.city, COUNT(c2.store_id) AS custumers
FROM customer c2
INNER JOIN store s2 ON s2.store_id = c2.store_id
INNER JOIN staff s ON s.staff_id = s2.manager_staff_id 
INNER JOIN address a ON s.address_id = a.address_id
INNER JOIN city c ON c.city_id = a.city_id
GROUP BY c2.store_id
HAVING COUNT(c2.store_id) > 300;
```

</details>

---

---

<details>
   <summary> Задание 2: </summary>
Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.
</details>

<details>
   <summary> Ответ 2: </summary>
  
```sql
SELECT COUNT(f.title) 
FROM film f
WHERE f.`length` > (SELECT AVG(`length`) FROM film)
```

</details>

---

---

<details>
   <summary> Задание 3: </summary>
Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

</details>

<details>
   <summary> Ответ 3: </summary>
  
```sql
SELECT *, DATE_FORMAT(date, '%d.%m.%Y') as new_date FROM employees, SUM(p.amount), COUNT(p.rental_id) 
FROM payment p
GROUP BY MONTH(payment_date)
ORDER BY SUM(p.amount ) DESC
LIMIT 1;
```

</details>

---

---