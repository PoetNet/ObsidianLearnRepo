# [Установка базы данных ClickHouse в виде контейнера Docker](https://www.youtube.com/watch?v=Zla6iN7W5ns)



- [Документация Clichouse](https://clickhouse.com/docs/ru)
- Документация образа: [yandex/clickhouse-server](https://hub.docker.com/r/yandex/clickhouse-server)

1. Запускаем Clickhouse в контейнере:
	`docker run -d --name some-clickhouse-server -p 8123:8123 --ulimit nofile=262144:262144 yandex/clickhouse-server`

или:
````bash
docker run --rm -e CLICKHOUSE_DB=db_etl -e CLICKHOUSE_USER=ofg -e CLICKHOUSE_DEFAULT_ACCESS_MANAGEMENT=1 -e CLICKHOUSE_PASSWORD=ofg -p 8123:8123 clickhouse/clickhouse-server
````
**Загрузка тестовых данных в Docker контейнер** (Находим тестовые данные по [ссылке](https://clickhouse.com/docs/ru/getting-started/example-datasets/metrica))
1. Смотрим контейнеры:
	`docker ps`
2. Входим в терминал контейнера
	`docker exec -it container_id bin/bash`
3.  `apt update && apt install curl`
4. Следуем инструкциям из [ссылки](https://clickhouse.com/docs/ru/getting-started/example-datasets/metrica)
5. 