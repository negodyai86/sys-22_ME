# `Домашнее задание к занятию "SQL. Часть 1"` - `Мешочков Александр SYS-22`



1. [Описание домашнего задания к занятию «SQL. Часть 1»](https://github.com/netology-code/sdb-homeworks/blob/main/12-03.md)

---
<details>
   <summary> Задание 1: </summary>
Получите уникальные названия районов из таблицы с адресами, которые начинаются на “K” и заканчиваются на “a” и не содержат пробелов.
</details>

<details>
   <summary> Ответ 1: </summary>
  
```sql
SELECT DISTINCT district
FROM address
WHERE district LIKE 'K%a' AND district NOT LIKE '% %';
```

</details>

---

<details>
   <summary> Задание 2: </summary>
Получите из таблицы платежей за прокат фильмов информацию по платежам, которые выполнялись в промежуток с 15 июня 2005 года по 18 июня 2005 года **включительно** и стоимость которых превышает 10.00.
</details>

<details>
   <summary> Ответ 2: </summary>
  
```sql
SELECT amount, payment_date
FROM payment
WHERE CAST(payment_date AS DATE) BETWEEN 20050614 AND 20050618 AND amount > 10.00;
```

</details>

---

<details>
   <summary> Задание 3: </summary>
Получите последние пять аренд фильмов.
</details>

<details>
   <summary> Ответ 3: </summary>
  
```sql
SELECT *
FROM rental
ORDER BY rental_id DESC
LIMIT 5;
```

</details>

---

<details>
   <summary> Задание 4: </summary>
Одним запросом получите активных покупателей, имена которых Kelly или Willie. 

Сформируйте вывод в результат таким образом:
- все буквы в фамилии и имени из верхнего регистра переведите в нижний регистр,
- замените буквы 'll' в именах на 'pp'.
</details>

<details>
   <summary> Ответ 4: </summary>
  
```sql
SELECT LOWER(REPLACE(first_name, 'LL', 'PP')) AS Имя, LOWER(last_name) AS Фамилия
FROM customer
WHERE active = 1 AND (first_name LIKE 'Kelly' OR first_name LIKE 'Willie');
```

</details>

---

