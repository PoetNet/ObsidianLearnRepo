**RabbitMQ** — распределённый и горизонтально масштабируемый брокер сообщений. Упрощённо его устройство можно описать так:

- publisher, который отправляет сообщения;
- queue, где хранятся сообщения;
- подписчики, которые выступают получателями сообщений.

RabbitMQ передаёт сообщения между поставщиками и подписчиками через очереди. Сообщения могут содержать любую информацию, например, о событии, произошедшем на сайте. 

RabbitMQ отлично подходит для интеграции разных компонентов, создания микросервисов, потоковой передачи данных в режиме реального времени или при передаче работы удалённым работникам. Его используют крупные компании, в числе которых Bloomberg, Reddit, WeWork, NASA и др.

#### **Почему выбирают RabbitMQ**:

- RabbitMQ поддерживает несколько протоколов: AMQP, MQTT, STOMP и др., что позволяет использовать его в разных сценариях.

- RabbitMQ хранит сообщение до тех пор, пока принимающее приложение не подключится и не получит его из очереди. Клиент может подтвердить получение сообщения сразу или после того, как полностью обработает его. Как только такое подтверждение получено, сообщение удаляется из очереди. Для сравнения в Kafka очередь сообщений является постоянной — данные хранятся, пока не истечёт указанный период или не будет достигнуто ограничение по размеру. Поэтому важно убедиться, что событие, которое должно произойти один раз, не воспроизводится многократно. 

- Основное преимущество RabbitMQ — гибкая маршрутизация. Сообщения маршрутизируются через exchange (обменник) перед попаданием в очереди. RabbitMQ предлагает несколько встроенных типов обмена для типичной логики маршрутизации. 

- RabbitMQ поддерживает приоритезацию в очередях и позволяет настроить диапазон приоритетов. Приоритет каждого сообщения устанавливается при его публикации. 

- RabbitMQ предлагает простой пользовательский интерфейс управления. Он позволяет контролировать каждый аспект брокера сообщений. .


### Сценарии использования RabbitMQ

Обычно отмечают два варианта, когда стоит использовать RabbitMQ: для фоновой обработки данных и для интеграции внутри приложений и между ними, т.е. в качестве брокера сообщений между микросервисами. Рассмотрим оба сценария. 

#### Фоновая обработка данных

Фоновая обработка данных — оптимальный сценарий использования RabbitMQ. Вы можете поместить сообщение в очередь без его немедленной обработки.

Пример: вы хотите загрузить отчёт из приложения. Приложение обрабатывает информацию и генерирует PDF-файл в течение 15-20 минут. Затем отправляет его вам по электронной почте. Очередь сообщений позволяет выполнить все задачи асинхронно.

Очереди RabbitMQ служат шинами событий и позволяют веб-серверам быстро реагировать на запросы вместо того, чтобы выполнять трудоемкие задачи на месте.

#### Посредник в микросервисной архитектуре

Также RabbitMQ часто используется для микросервисной архитектуры, где он выступает средством связи между приложениями и помогает избегать узких мест при передаче сообщений.

### Ещё пара слов о RabbitMQ

Когда приложение ломается, другие приложения уже не могут обмениваться с ним сообщениями, поэтому работа останавливается. RabbitMQ же кладёт сообщение в очередь, дожидается, пока сломанное приложение починят, и отдаёт ему сообщение из очереди. Да, есть некоторая задержка на время устранения причины поломки, но глобально ничего не падает. 

Также RabbitMQ выполняет распределённые нагрузки. Несколько копий одного приложения могут быть запущены на разных компьютерах. RabbitMQ распределит нагрузку относительно производительности и выберет, кому отправить сообщение. 

RabbitMQ — больше, чем просто брокер сообщений. Он основан на ERLANG и платформе Open Telecom, обеспечивающей высокую производительность при использовании минимальных ресурсов. Его выбирают, когда требуется надёжная доставка сообщений и гибкая маршрутизация.