Habr: [Разбираем TLS по байтам. Кто такой этот HTTPS? / Хабр (habr.com)](https://habr.com/ru/companies/timeweb/articles/752270/)

---

`HTTPS` (HyperText Transfer Protocol Secure) — расширение протокола [[HTTP|HTTP]] для поддержки шифрования в целях повышения безопасности. Данные в протоколе HTTPS передаются поверх криптографических протоколов [[TLS, SSL|TLS и SSL]]. В отличие от [[HTTP|HTTP]] с [[TCP, UDP|TCP]]-портом 80, для `HTTPS` по умолчанию используется [[TCP, UDP|TCP]]-порт 443.

`HTTPS` не является отдельным протоколом. Это обычный HTTP, работающий через шифрованные транспортные механизмы [[TLS, SSL|SSL]] и [[TLS, SSL|TLS]]. Он обеспечивает защиту от атак, основанных на прослушивании сетевого соединения — от атак и атак типа `man-in-the-middle`, при условии, что будут использоваться шифрующие средства и сертификат сервера проверен и ему доверяют.

Чтобы подготовить веб-сервер для обработки `https-соединений`, администратор должен получить и установить в систему сертификат открытого и закрытого ключа для этого веб-сервера. В [[TLS, SSL|TLS]] используется как `асимметричная схема шифрования` (для выработки общего секретного ключа), так и `симметричная` (для обмена данными, зашифрованными общим ключом). Сертификат открытого ключа подтверждает принадлежность данного открытого ключа владельцу сайта. Сертификат открытого ключа и сам открытый ключ посылаются клиенту при установлении соединения, а закрытый ключ используется для расшифровки сообщений от клиента.

Существует возможность создать такой сертификат, не обращаясь в центр сертификации. Подписываются такие сертификаты этим же сертификатом и называются самоподписанными (self-signed). Без проверки сертификата каким-то другим способом (например, звонок владельцу и проверка контрольной суммы сертификата) такое использование `HTTPS` подвержено атаке посредника.

Эта система также может использоваться для аутентификации клиента, чтобы обеспечить доступ к серверу только авторизованным пользователям. Для этого администратор обычно создаёт сертификаты для каждого пользователя и загружает их в браузер каждого пользователя. Также будут приниматься все сертификаты, подписанные организациями, которым доверяет сервер. Такой сертификат обычно содержит имя и адрес электронной почты авторизованного пользователя, которые проверяются при каждом соединении, чтобы проверить личность пользователя без ввода пароля.

В `HTTPS` для шифрования используется длина ключа 40, 56, 128 или 256 бит. 

### Идентификация в HTTPS

#### Идентификация сервера

[[HTTP|HTTP]]/[[TLS, SSL|TLS]] запросы генерируются путём разыменования URI, вследствие чего имя хоста становится известно клиенту. В начале общения, сервер посылает клиенту свой сертификат, чтобы клиент идентифицировал его. Это позволяет предотвратить атаку посредника. В сертификате указывается URI сервера. Согласование имени хоста и данных, указанных в сертификате, происходит в соответствии с протоколом `RFC2459`.

Если имя сервера не совпадает с указанным в сертификате, то пользовательские программы, например браузеры, сообщают об этом пользователю. В основном, браузеры предоставляют пользователю выбор: продолжить незащищённое соединение или прервать его.

### Идентификация клиента

Обычно сервер не располагает информацией о клиенте, достаточной для его идентификации. Однако для обеспечения повышенной защищённости соединения используется так называемая `two-way authentication`. При этом сервер после подтверждения его сертификата клиентом также запрашивает сертификат. Таким образом, схема подтверждения клиента аналогична идентификации сервера.

### Атаки с использованием анализа трафика
Основная статья: **[Атака с использованием анализа трафика](https://ru.wikipedia.org/wiki/%D0%90%D1%82%D0%B0%D0%BA%D0%B0_%D1%81_%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5%D0%BC_%D0%B0%D0%BD%D0%B0%D0%BB%D0%B8%D0%B7%D0%B0_%D1%82%D1%80%D0%B0%D1%84%D0%B8%D0%BA%D0%B0 "Атака с использованием анализа трафика")**

В HTTPS были обнаружены уязвимости, связанные с анализом трафика. `Атака с анализом трафика` — это тип атаки, при которой выводятся свойства защищённых данных канала путём измерения размера трафика и времени передачи сообщений в нём. Анализ трафика возможен, поскольку шифрование [[TLS, SSL|TLS/SSL]] изменяет содержимое трафика, но оказывает минимальное влияние на размер и время прохождения трафика. 
### Атака посредника

[![](https://upload.wikimedia.org/wikipedia/commons/thumb/9/91/Mim8989.png/428px-Mim8989.png)](https://commons.wikimedia.org/wiki/File:Mim8989.png?uselang=ru)
Рис.1 Стандартная конфигурация сети. Пользователь на хосте клиента (CH) хочет осуществить безопасную транзакцию, но уязвим для атаки «человек в середине».

Основная статья: **[Атака посредника](https://ru.wikipedia.org/wiki/%D0%90%D1%82%D0%B0%D0%BA%D0%B0_%D0%BF%D0%BE%D1%81%D1%80%D0%B5%D0%B4%D0%BD%D0%B8%D0%BA%D0%B0 "Атака посредника")**

При «атаке посредника» используется то, что сервер `HTTPS` отправляет сертификат с открытым ключом в браузер. Если этот сертификат не заслуживает доверия, то канал передачи будет уязвимым для атаки злоумышленника. Такая атака заменяет оригинальный сертификат, удостоверяющий `HTTPS-сервер`, модифицированным сертификатом. Атака проходит успешно, если пользователь пренебрегает двойной проверкой сертификата, когда браузер отправляет предупреждение. Это особенно распространено среди пользователей, которые часто сталкиваются с самозаверенными сертификатами при доступе к сайтам внутри сети частных организаций.

На рис. 1 представлена ситуация, когда злоумышленник является шлюзом между клиентом, осуществляющим безопасную транзакцию, и сервером. Таким образом через злоумышленника проходит весь трафик клиента и он может перенаправить его по своему усмотрению. Здесь делаются следующие шаги:

1. Злоумышленник встраивается между клиентом и сервером.
2. Пересылает все сообщения от клиента серверу без изменений.
3. Перехват сообщений от сервера, посланных по шлюзу по умолчанию.
4. Создание самозаверенного сертификата и подмена им сертификата сервера.
5. Отправление ложного сертификата клиенту.
6. Когда клиент подтвердит сертификат, будут установлены защищённые соединения: между злоумышленником и сервером и другое — между злоумышленником и клиентом.

В итоге такой атаки клиент и сервер думают, что осуществляют безопасное соединение, однако злоумышленник теперь также имеет закрытый ключ и может расшифровать любое сообщение в канале.
