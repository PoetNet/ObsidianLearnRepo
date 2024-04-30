Тема на Stepik: 4.7 https://stepik.org/lesson/840444/step/1?unit=844089.

Подзапрос в условии WHERE:

```PostgreSQL
SELECT *
	FROM band
	WHERE band_id IN (SELECT id
						FROM list1
					 )
```

Подзапрос в части FROM (вложенный SELECT) с использованием алиаса:

```PostgreSQL
SELECT *
	FROM 
		(SELECT band_id, name
			FROM band
		 WHERE year >= 1980
		 ) as p
	WHERE p.band_id BETWEEN 10 AND 20;
```
