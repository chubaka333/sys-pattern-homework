# Домашнее задание к занятию 12.5. «Индексы» - Неудахин Денис

### Задание 1

Напишите запрос к учебной базе данных, который вернёт процентное отношение общего размера всех индексов к общему размеру всех таблиц.

<img src = "img/1.PNG" width = 100%>

### Задание 2

Выполните explain analyze следующего запроса:
```sql
select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
```
- перечислите узкие места;
- оптимизируйте запрос: внесите корректировки по использованию операторов, при необходимости добавьте индексы.

Запросы вызывает таблицы целиком, каждое такое полное присоединение занимает по пол секунды, особенно долго занимает сортировка c.customer_id, f.title по сути 2/3 времени запроса.

<img src = "img/2-1.PNG" width = 100%>

Оригинальный результат

<img src = "img/2-2.PNG" width = 100%>

Работа изменённого запроса

<img src = "img/2-3.PNG" width = 100%>

Время работы изменённого запроса 31 мс вместо 5 секунд оригинального запроса.

