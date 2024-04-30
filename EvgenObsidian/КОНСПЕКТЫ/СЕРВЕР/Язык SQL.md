**SQL** (Structured Query Language)  — декларативный язык программирования, применяемый для создания, модификации и управления данными в реляционной базе данных, управляемой соответствующей. В общем случае SQL (без ряда современных расширений) считается языком программирования неполным по Тьюрингу, но вместе с тем стандарт языка спецификацией [SQL/PSM](https://ru.wikipedia.org/wiki/SQL/PSM) предусматривает возможность его процедурных расширений.

#### Операторы SQL делятся на:

1. Операторы определения данных (`Data Definition Language`):
    - CREATE - создаёт объект базы данных ,
    - ALTER - изменяет объект,
    - DROP - удаляет объект;
2. Операторы манипуляции данными (`Data Manipulation Language`):
    - SELECT - выбирает данные, удовлетворяющие заданным условиям,
    - INSERT - добавляет новые данные,
    - UPDATE - изменяет существующие данные,
    - DELETE - удаляет данные;
3. Операторы определения доступа к данным (`Data Control Language`):
    - GRANT предоставляет пользователю (группе) разрешения на определённые операции с объектом,
    - REVOKE отзывает ранее выданные разрешения,
    - DENY задаёт запрет, имеющий приоритет над разрешением;
4. Операторы управления транзакциями (`Transaction Control Language`):
    - COMMIT - применяет транзакцию,
    - ROLLBACK - откатывает все изменения, сделанные в контексте текущей транзакции,
    - SAVEPOINT - делит транзакцию на более мелкие участки.  

#### Примеры:

```PostgreSQL
SELECT * 
FROM todoitem
WHERE 
	status = false
OFFSET 5
LIMIT 10;
```

```PostgreSQL
UPDATE todoitem
SET 
	status = true
WHERE 
	id = 1	
```

```PostgreSQL
DELETE FROM todoitem
WHERE 
	status = true
```

```PostgreSQL
SELECT *
FROM 
	todoitem AS td
	INNER JOIN users AS u ON u.id = td.owner_id
```

```PostgreSQL
SELECT 
	b.name, b.year 
FROM 
	band AS b
	JOIN album AS a ON b.band_id = a.band_id
```

### Создание пользователя в СУБД

```PostgreSQL
CREATE USER myuser WITH ENCRYPTED PASSWORD 'mypassword' SUPERUSER;
```

### Создание БД 
```PostgreSQL
CREATE DATABASE имя_базы_данных
  WITH
    OWNER = имя_владельца,
    ENCODING = 'кодировка'
    CONNECTION LIMIT = -1
```

### Выделение прав пользователю 
```PostgreSQL
GRANT ALL PRIVILEGES ON DATABASE имя_базы_данных TO имя_пользователя;
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO myuser;
```

### Подключение к БД
```bash
psql -U имя_пользователя -d имя_базы_данных -h хост

psql -U myuser -d mydatabase -h localhost
```
### Создание таблицы
```PostgreSQL
CREATE TABLE todoItem (
    CreatedAt     TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    Id            BIGSERIAL PRIMARY KEY,
    Status        BOOLEAN DEFAULT false,
    Text          TEXT
);

INSERT INTO todoItem (Text)
VALUES
    ('Создать план на следующую неделю'),
```