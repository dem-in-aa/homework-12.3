# Домашнее задание к занятию «SQL. Часть 1» Андрей Дёмин



### Задание 1

Получите уникальные названия районов из таблицы с адресами, которые начинаются на “K” и заканчиваются на “a” и не содержат пробелов.
```
SELECT DISTINCT district FROM address WHERE district LIKE 'K%a' AND district NOT LIKE '% %';
```

### Задание 2

Получите из таблицы платежей за прокат фильмов информацию по платежам, которые выполнялись в промежуток с 15 июня 2005 года по 18 июня 2005 года **включительно** и стоимость которых превышает 10.00.
```
SELECT * FROM payment WHERE payment_date BETWEEN '2005-06-15 00:00:00' AND '2005-06-18 23:59:59' AND amount>10;
```
с добавлением сортировки по дате платежа:
```
SELECT * FROM payment WHERE payment_date BETWEEN '2005-06-15 00:00:00' AND '2005-06-18 23:59:59' AND amount>10 order by payment_date;
```

### Задание 3

Получите последние пять аренд фильмов.
```
SELECT * FROM rental ORDER BY  rental_date DESC LIMIT 5;
```

### Задание 4

Одним запросом получите активных покупателей, имена которых Kelly или Willie. 

Сформируйте вывод в результат таким образом:
- все буквы в фамилии и имени из верхнего регистра переведите в нижний регистр,
- замените буквы 'll' в именах на 'pp'.
```
SELECT LOWER(last_name) , LOWER(REPLACE(first_name, 'LL', 'PP')) FROM customer WHERE first_name LIKE 'Kelly' OR first_name LIKE 'Willie';
```

### Задание 5*

Выведите Email каждого покупателя, разделив значение Email на две отдельных колонки: в первой колонке должно быть значение, указанное до @, во второй — значение, указанное после @.
```
SELECT SUBSTRING_INDEX(email, '@', 1) as user_name,
RIGHT (email, (CHAR_LENGTH(email) - CHAR_LENGTH(SUBSTRING_INDEX(email, '@', 1)) -1))
as domain_name FROM customer LIMIT 10;
```

### Задание 6*

Доработайте запрос из предыдущего задания, скорректируйте значения в новых колонках: первая буква должна быть заглавной, остальные — строчными.
```
SELECT 
CONCAT(UPPER(LEFT (1column, 1)), LOWER(RIGHT(1column, (CHAR_LENGTH(1column)-1)))) as user_name,
CONCAT(UPPER(LEFT (2column, 1)), LOWER(RIGHT(2column, (CHAR_LENGTH(2column)-1)))) as domain_name
FROM
(SELECT SUBSTRING_INDEX(email, '@', 1) as 1column,
RIGHT (email, (CHAR_LENGTH(email) - CHAR_LENGTH(SUBSTRING_INDEX(email, '@', 1)) -1)) as 2column
FROM customer) t;
```
