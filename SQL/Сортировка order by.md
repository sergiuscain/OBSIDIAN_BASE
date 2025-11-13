## -- Сортировка order by
### ASC-во возрастанию (самые маленькие значения первые (вверху таблицы))
### DESC-по убыванию. 
```sql
SELECT * FROM orders ORDER BY order_date DESC;
```
Можно сортировать по нескольким параметрам.
(Запрос ниже вернет все строки из таблицы apartments, отсортированные по столбцу num_rooms в порядке возрастания, а затем по столбцу price в порядке убывания)
```sql
SELECT * FROM apartments ORDER BY num_rooms, price DESC;
```