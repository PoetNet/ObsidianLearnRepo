
Создание базы `todo`:

```PostgreSQL
CREATE DATABASE todo
    WITH
    OWNER = postgres
    ENCODING = 'UTF8'
    CONNECTION LIMIT = -1
    IS_TEMPLATE = False;
```

Создание таблицы `todoItem`:

```PostgreSQL
CREATE TABLE todoItem (
    CreatedAt     TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    Id            BIGSERIAL PRIMARY KEY,
    Status        BOOLEAN DEFAULT false,
    Text          TEXT,
    UpdatedAt     TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
```

Заполнение таблицы значениями:

```PostgreSQL
INSERT INTO todoItem (Text)
VALUES
    ('Создать план на следующую неделю'),
    ('Прочитать новую книгу по программированию'),
    ('Записаться на курс по управлению временем'),
    ('Посетить тренировку в спортзале'),
    ('Найти и изучить новый язык программирования'),
    ('Подготовить план обедов на эту неделю'),
    ('Провести аудиторию по текущим проектам'),
    ('Улучшить навыки работы с базой данных'),
    ('Записаться на вебинар по разработке веб-приложений'),
    ('Пройти тестирование новой версии программного обеспечения'),
    ('Сделать 30-минутную прогулку каждый день'),
    ('Найти и прочитать интересную статью по своей теме'),
    ('Обновить резюме'),
    ('Изучить новый метод управления проектами'),
    ('Найти и применить новый дизайн-паттерн в проекте'),
    ('Написать план обучения на следующий месяц'),
    ('Подготовиться к собеседованию'),
    ('Провести код-ревью у коллеги'),
    ('Найти и прочитать интересную статью по своей теме'),
    ('Написать статью на тему последних технологических трендов'),
    ('Подготовить презентацию для конференции');
```

Делам поле `updatetime` автообновляемым при каждом изменении записи:

```PostgreSQL
-- Создаем функцию, которая будет вызываться перед обновлением записи
CREATE OR REPLACE FUNCTION set_update_time()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updatedAt := NOW(); -- Устанавливаем новое значение для updateTime
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Создаем триггер, связанный с функцией
CREATE TRIGGER trigger_set_update_time
BEFORE UPDATE ON todoItem
FOR EACH ROW
EXECUTE FUNCTION set_update_time();
```
