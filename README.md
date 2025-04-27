# Домашнее задание к занятию "`SQL2`" - `Бедник Денис`


---

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

```sql
SELECT 
   st.last_name, 
   st.first_name, 
   ci.city,
   COUNT(c.customer_id) customer_count
FROM 
   store s 
JOIN
   staff st ON s.store_id = st.store_id
JOIN 
   customer c ON s.store_id = c.store_id
JOIN 
   address a ON s.address_id = a.address_id
JOIN 
   city ci ON a.city_id = ci.city_id
GROUP BY 
   st.staff_id
HAVING customer_count > 300 
```


---

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

```sql
SELECT COUNT(*)
FROM film
WHERE length > (SELECT AVG(length) FROM film);
```

---

### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

```sql
SELECT DATE_FORMAT(p.payment_date, '%m - %Y') pay_m, 
       SUM(p.amount) total_payments,
       COUNT(p.rental_id) summ_rental
FROM payment p
GROUP BY pay_m
ORDER BY total_payments DESC
LIMIT 1;
```


--- 

### Задание 4

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

```sql
SELECT 
		COUNT(payment_id) count_pay,
CASE
		WHEN COUNT(payment_id) > 8000 THEN 'Да'
		ELSE 'Нет'
END AS Премия
FROM
		payment p
GROUP BY 
		staff_id;
```      

---


### Задание 5

Найдите фильмы, которые ни разу не брали в аренду.

```sql
SELECT 
		f.title
FROM 
		film f
LEFT JOIN 
		inventory i ON i.film_id = f.film_id
LEFT JOIN 
		rental r ON r.inventory_id = i.inventory_id
WHERE 
		r.rental_id IS NULL;
```