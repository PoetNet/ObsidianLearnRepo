Тема на Stepik: 4.6 https://stepik.org/lesson/840443/step/1?unit=844088.

Если нужно получить выборку групп, где id = 336, 93, 192, то вместо такого запроса:

```PostgreSQL
SELECT band_id, name
	FROM band
	WHERE
		band_id = 336
	 OR band_id = 93
	 OR band_id = 192;
```

Можно использовать ключевое слово IN, которое проверяет совпадение с элементами последовательности.

```PostgreSQL
SELECT band_id, name
	FROM band
	WHERE
		band_id IN (336, 93, 192)
```

Аналогично работает  NOT IN. Выберем все группы, id которых не равны 2, 92 и 333:

```PostgreSQL
SELECT band_id, name
	FROM band
	WHERE
		band_id NOT IN (2, 92, 333)
```

Если нужно сделать выборку по диапазону значений, то можно использовать ключевое слова BETWEEN:

```PostgreSQL
SELECT band_id, name, year
	FROM band
	WHERE
		year BETWEEN 1970 and 1981;
```

Оператор NOT инвертирует условие:

```PostgreSQL
SELECT * FROM band
	WHERE
		NOT (band_id = 2);

-- Этот же запрос без NOT
SELECT * FROM band
	WHERE
		band_id <> 2;
```
