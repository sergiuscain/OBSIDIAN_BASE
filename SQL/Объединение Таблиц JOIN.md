##  Объединение Таблиц
### 1) INNER JOIN (Внутреннее соединение)
Возвращает только строки, где есть совпадение в обеих таблицах.
```sql
SELECT users.name, orders.total
FROM users
INNER JOIN orders ON users.id = orders.user_id;
```
### 2) LEFT JOIN (Левое внешнее соединение)
Возвращает все строки из левой таблицы + совпадения из правой. Если совпадений нет - NULL.
```sql
-- Все пользователи и их заказы (даже если заказов нет)
SELECT users.name, orders.total
FROM users
LEFT JOIN orders ON users.id = orders.user_id;
```
### 3) RIGHT JOIN (Правое внешнее соединение)
Обратный LEFT JOIN. Все строки из правой таблицы + совпадения из левой.
```sql
-- Все заказы и пользователи (даже если пользователь удалён)
SELECT users.name, orders.total
FROM users
RIGHT JOIN orders ON users.id = orders.user_id;
```
 ### 4) FULL JOIN (Полное соединение)
Возвращает все строки из обеих таблиц.
```sql
SELECT o.order_id, o.order_date, c.customer_name 
FROM orders AS o 
FULL JOIN customers c ON o.customer_id = c.customer_id
```
### Шпаргалка Join
![[SQLJOIN.jpg]]