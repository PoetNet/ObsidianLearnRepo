#Pre-exam #BaseServer
## 1. Клиент-серверная архитектура.

[[Клиент-серверная архитектура]]
## 2. HTTP Codes

[[HTTP]]
1xx - Информационные 
• 102 - запрос принят на обработку, клиент ждет

2хх - Успех
• 200 - ОК
• 201 - CREATED
• 202 - ACCEPTED клиент не ждет
• 203 - 200, но инфа из ненадежного и сточника
• 204 - ОК, но в ответе только заголовки
• 205 - Клиент должен сбросить введенные юзером данные

3хх - Перенаправление

• 307 - временное перенаправление (Location)
• 308 - постоянное перенаправление

4хх - Ошибка клиента
• 400 - BAD REQUEST неправильный синтаксис
• 401 - UNAUTHORIZED
• 402 - PAYMENT REQUE ----
• 403 - FORBIDDEN
• 404 - NOT FOUND
• 405 - METHOD NOT ALLOWED 
• 418 - IM TEAPOD (Реальные кейсы применения?)
• 429 - TOO MANY REQUESTS 

5xx - Ошибка сервера
• 500 - Любая ошибка сервера
• 501 - NOT IMPLEMENTED
• 502 - BAD GATEWAY
• 503 - SERVICE UNAVAILABLE
• 504 - GATEWAY TIMEOUT
• 505 - HTTP VERSION NOT SUPPORTED

## 3. HTTPS  

[[HTTPS]]
## 4. WebSockets

[[Web Sockets]]
## 5. REST

[[REST]]
Рассмотри **6 принципов** REST — ограничений, которые и помогают нам добиться этих нефункциональных требований.

1. Клиент-серверная архитектура
2. Stateless
3. Кэширование
4. Единообразие интерфейса
5. Layered system
6. Code on demand

#### Принцип 1. Клиент-серверная архитектура

#### Принцип 2. Stateless

Принцип заключается в том, что сервер не должен хранить у себя информацию о сессии с клиентом. Он должен в каждом запросе получать всю информацию для обработки.

>Какие он даёт плюсы?
- Масштабируемость сервера,
- Уменьшение времени обработки запроса,
- Простота поддержки,
- Возможность использовать кэширование.

>Какие проблемы может создать Stateless-подход?
- Усложнение логики клиента (именно на стороне клиента нам нужно хранить всю информацию о состоянии, о допустимых действиях, о недопустимых действиях и подобных вещах).
- Увеличение нагрузки на сеть (каждый раз мы передаём всю информацию, весь контекст. Таким образом, больше информации гоняем по сети).
#### Принцип 3. Кэширование

>Какие у кэширования плюсы?
- Уменьшение количества сетевых взаимодействий.
- Уменьшение нагрузки на системы (не грузим их дополнительными запросами).

При этом важно понимать, что кэширование — это совсем не простая штука. Она бывает достаточно сложна и нетривиальна в реализации.  
Также мы должны учитывать, что если отдаём какие-то данные, которые сохранили раньше, то важно помнить, что эти данные могли уже устареть.

#### Принцип 4. Единообразие интерфейса. HATEOAS

`Hypermedia as the Engine of Application State` (`HATEOAS`) — одно из ограничений `REST`, согласно которому сервер возвращает не только ресурс, но и его связи с другими ресурсами и действия, которые можно с ним совершить.

>Главный плюс этого подхода — клиент становится очень гибким в плане изменений на сервере с точки зрения изменения допустимых действий, изменения модели данных и т.д.

>В качестве обратной стороны медали мы получаем сильное усложнение логики, в первую очередь, клиента.

#### Принцип 5. Layered system (слоистая архитектура)

Концепция слоистой архитектуры заключается в том, что ни клиент, ни сервер не должны знать о том, как происходит цепочка вызовов дальше своих прямых соседей.

>Минусы:
- Увеличение нагрузки на сеть (больше участников и больше вызовов, чем если бы мы шли один раз от клиента до сервера напрямую).
- Увеличение времени получения ответа (из-за появления дополнительных участников).

#### Принцип 6. Code on done (код по требованию)

Идея передачи некоторого исполняемого кода (по сути какой-то программы) от сервера клиенту.

#### **Преимущества REST-архитектуры:**

1. **Простота:** `REST` использует стандартные `HTTP-методы` и легко понимаемые структуры данных, что делает его простым для разработчиков.
2. **Масштабируемость:** `RESTful` сервисы могут быть легко масштабированы, так как они не хранят состояние между запросами.
3. **Гибкость:** Клиенты и серверы взаимодействуют через стандартные протоколы и форматы, что обеспечивает гибкость и возможность изменения без воздействия на другие компоненты системы.
4. **Кэширование:** `REST` поддерживает кэширование данных, что может существенно улучшить производительность.

**Пример:** GET /users/{id}

## 6. SOAP, GPARHQL, gRPC

[[SOAP]]
[[GraphQL]]
[[RPC, gRPC|gRPC]]

## 7. NOSQL DATABASES

1. **Ключ-значение (Key-Value Stores):**
    - _Примеры:_ Redis, DynamoDB
    - _Плюсы:_ Простота, высокая производительность при операциях чтения и записи, гибкость в моделировании данных.
    - _Минусы:_ Ограниченные возможности запросов, сложности при выполнении сложных запросов, если требуется извлечение данных по условиям, отличным от ключа.

2. **Документно-ориентированные (Document Stores):**
    - _Примеры:_ MongoDB.
    - _Плюсы:_ Способность хранить сложные документы, удобные для использования в приложениях.
    - _Минусы:_ Возможны проблемы с производительностью при запросах, требующих объединения данных из разных документов.

3. **Столбцово-ориентированные (Column-Family Stores):**
    - _Примеры:_ Apache Cassandra.
    - _Плюсы:_ Эффективность при операциях чтения и записи для больших объемов данных, хорошая масштабируемость.
    - _Минусы:_ Ограниченные возможности запросов.

4. **Графовые (Graph Databases):**
       - _Примеры:_ Amazon Neptune, ArangoDB.
    - _Плюсы:_ представление и обработка связей между данными, удобные для решения задач, связанных с анализом сетей и графов.
    - _Минусы:_ Ограниченные в некоторых сценариях.

5. Поисковые
	- **Индексация:** Данные индексируются для быстрого и эффективного поиска. 
	- Функции поиска.
Примеры поисковых баз данных включают `Elasticsearch`.

## 8. SQL
SQL (Structured Query Language) - это язык структурированных запросов, используемый для работы с реляционными базами данных.

SQL-команды делятся на несколько категорий:

1. `DML` (Data Manipulation Language) - Язык управления данными. Он используется для управления данными внутри таблиц баз данных. Команды DML включают `SELECT`, `INSERT`, `UPDATE` и `DELETE`.

2. `DDL` (Data Definition Language) - Язык определения данных. Он используется для управления структурой баз данных. Команды DDL включают `CREATE`, `ALTER` и `DROP`. Эти команды позволяют создавать, изменять и удалять базы данных, таблицы и другие объекты баз данных.

3. `DCL` (Data Control Language) - Язык управления доступом к данным. Он используется для управления правами доступа к данным. Команды DCL включают `GRANT` и `REVOKE`, которые предоставляют или отменяют разрешения на выполнение определенных операций с базой данных или ее объектами.

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

## 10. Создание пользователя в СУБД

```PostgreSQL
CREATE USER myuser WITH ENCRYPTED PASSWORD 'mypassword' SUPERUSER;
// create user --interactive       Другие СУБД
```

## 11. Создание БД 
```PostgreSQL
CREATE DATABASE имя_базы_данных
  WITH
    OWNER = имя_владельца,
    ENCODING = 'кодировка'
    CONNECTION LIMIT = -1

// create database --interactive   Другие СУБД
```

## 12. Выделение прав пользователю 
```PostgreSQL
GRANT ALL PRIVILEGES ON DATABASE имя_базы_данных TO имя_пользователя;
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO myuser;
```

## 13. Подключение к БД
```bash
psql -U имя_пользователя -d имя_базы_данных -h хост

psql -U myuser -d mydatabase -h localhost
```
## 14. Создание таблицы
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

## 15. Ключи БД
Все значения поля band_id должны быть уникальными и не равны null. Такое условие называется условием `первичного ключа`.

В таблице `album` все значения, которые есть в  поле `band_id` должны существовать в таблице `band`. Это условие внешнего ключа.

## 18. Сгенерировать новый веб проект

```C#
dotnet new web -n MyWebApp
cd MyWebApp
dotnet run
```
## 19. ENV
`.env` файл - это текстовый файл, который содержит пары "ключ-значение" для переменных окружения. Пример:
```env
DB_CONNECTION="Host=localhost;Port=PORT;Database=DBNAME;Username=USER;Password=PASSWORD;"
```

```C#
DotNetEnv.Env.Load();
var connectionString = Environment.GetEnvironmentVariable("DB_CONNECTION");
```
**`Microsoft.Extensions.Configuration:`** Встроенная библиотека в ASP.NET Core.

## 20. Подключение обработчиков запросов.

```C#
var builder = WebApplication.CreateBuilder(args);   
var app = builder.Build();  
app.UseRouting();  
  
app.UseEndpoints(endpoints =>  
{  
    endpoints.MapGet("/", async context => await context.Response.WriteAsync("Text"));  
    endpoints.MapGet("/filename", async context => await context.Response.SendFileAsync(file));  
    endpoints.MapGet("/todo/{id}", async context =>  
    {  
        var response = GetTodo(id);  
        await context.Response.WriteAsync(JsonConvert.SerializeObject(response));  
    });  
    endpoints.MapPost("/todo",async context =>  
    {  
        using (var reader = new StreamReader(context.Request.Body))  
        {  
            var requestBody = await reader.ReadToEndAsync();  
            var todoData = JsonConvert.DeserializeObject<TodoDto>(requestBody);  
            CreateTodo(todoData.Status);  
            context.Response.StatusCode = 200;  
        }  
    });  
});
```

## 21. Указать методы для обработки запросов.

```C#
[Route("v1/todo")]
[ApiController]
public class TodoItemsController : ControllerBase  
    { 
        // GET  
        [HttpGet("{id}")]  
        public async Task<T> GetItem(params) {}    
```

## 22. HttpContext
`HttpContext` в ASP.NET Core представляет собой объект, который предоставляет доступ к информации о текущем HTTP-запросе и ответе в рамках обработки HTTP-запроса сервером. Он содержит данные о запросе, такие как URL, заголовки, параметры запроса, а также предоставляет доступ к объектам `HttpRequest` и `HttpResponse`.

1. Доступ к свойствам запроса (`Request`) и ответа (`Response`).
2. Управление сеансом пользователя (`Session`).
3. Передача данных между Middleware и компонентами обработки запроса.
4. Аутентификация и авторизация.
5. Управление маршрутизацией и обработкой исключений.

```csharp
public class MyController : ControllerBase
{
    public IActionResult MyAction()
    {
        HttpContext context = HttpContext;
    }
}
```

## 23. Обработка тела запроса.

```C#
[HttpPost("/api/todo")]
public IActionResult CreateTodo([FromBody] TodoDto dto)
{
	var item = _service.CreateTodo(dto.Text);
    return Ok(item);
}
```

#### Получение параметров из пути запроса (Route Parameters):

```C#
[HttpGet("/api/todo/{id}")]
public IActionResult GetTodoById(int id)
{
    return Ok($"Получен пользователь с ID {id}");
}
```
#### Получение значений Query Params:

```C#
[HttpGet]  
        public async Task<T> GetTodoItem([FromQuery] int page, [FromQuery] int perPage)  
        {  
            var todoItem = await _context.TodoItems  
                .Skip((page - 1) * perPage)  
                .Take(perPage)  
                .ToListAsync();  
            return OK();  
        }
```

## 24. Вернуть ответ

#### Возвращение результата с определенным статус-кодом и данными:
```C#
public IActionResult MyResultAction()
{
    var result = new
    {
        Status = "Success",
        Message = "Операция выполнена успешно"
    };

    return Ok(result); // 200 OK
    return StatusCode(500, value: "message");
}
```

#### Отправка данных через HTTP-заголовки:

```C#
public IActionResult MyHeaderAction()
{
    Response.Headers.Add("CustomHeader", "HeaderValue");
    return Ok("Данные с заголовком");
}

```

## 25. Entity Framework Core

1. Кросс-платформенность.
2. Поддержка различных баз данных.
3. Поддержка  (Code-First).
4. **Поддержка (Database-First).
5. **Поддержка миграций баз данных:** EF Core предоставляет механизм миграций, который позволяет обновлять схему базы данных с помощью кода.

```csharp
public class TodoDbContext : DbContext
{
    public DbSet<TodoItem> TodoItems { get; set; }

	public TodoDbContext(DbContextOptions<ToDoContext> options)
		:base(options)
	{
	}
	
	protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)  
	{  
		 optionsBuilder.UseNpgsql(connectionString);  
	}
}

using (var context = new TodoDbContext())
{
    var todo = new TodoItem { Text = "Pass Exam" };
    context.TodoItems.Add(todo);
    context.SaveChanges();
    
	var todoItem = await context.TodoItems
    .OrderBy(x => x.Id)
    .Skip((page - 1) * perPage)
    .Take(perPage)
    .ToListAsync();
}
```

## 26. Подключение к базе

   ```csharp
using Microsoft.EntityFrameworkCore;

connection = "Host=localhost;Port=PORT;Database=DBNAME;Username=USER;Password=PASSWORD;"
builder.Services.AddDbContext<ToDoContext>(options => options.UseNpgsql(connection));
   ```

4. **Использование контекста в коде:**
   Используйте контекст базы данных в коде для выполнения операций с базой данных.

   ```csharp
   using (var context = new TodoDbContext())
   {
       var user = new Todo { Text = "John Doe" };
       context.ToDoItems.Add(todo);
       context.SaveChanges();
   }
   ```

## 27.  DTO
DTO (Data Transfer Object) представляет собой объект, который используется для передачи данных между различными компонентами вашего приложения, особенно между серверным и клиентским кодом. 

### Как использовать DTO в серверных приложениях на C#:

1. **Определяем DTO класс:**

2. **Использование DTO в контроллерах ASP.NET:**
   ```csharp
   [HttpGet("todo/{id}")]
   public ActionResult<UserDTO> GetTodoItemById(int id)
   {
       var todoItem = _userService.GetTodoItemById(id);
       var todoDTO = new ToDoDTO
       {
           Id = user.Id,
           Name = user.Name,
           Email = user.Email
       };

       return userDTO;
   }
   ```

Пример с использованием `AutoMapper`:
   ```csharp
builder.Services.AddAutoMapper(typeof(AutoMapperProfile));

CreateMap<TodoItem, TodoItemDTO>();

var responseData = _mapper.Map<GetTodoDto>(todoItems);
   ```

## 28. JSON

### С использованием System.Text.Json

```csharp
using System.Text.Json;

//Сериализация в JSON:
var item = new { Text = "Read a book" };
string jsonString = JsonSerializer.Serialize(obj);

//Десериализация из JSON:
string jsonString = "{\"Name\":\"John\",\"Age\":30}";
var obj = JsonSerializer.Deserialize<MyClass>(jsonString);
```

### С использованием Newtonsoft.Json:

```csharp
using Newtonsoft.Json;

//Сериализация в JSON:
var obj = new { Text = "John", Age = 30 };
string jsonString = JsonConvert.SerializeObject(obj);

//Десериализация из JSON:
string jsonString = "{\"Name\":\"John\",\"Age\":30}";
var obj = JsonConvert.DeserializeObject<MyClass>(jsonString);
```
