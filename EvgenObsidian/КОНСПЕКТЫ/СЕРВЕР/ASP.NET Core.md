### .ENV file
`.env` файл - это текстовый файл, который содержит пары "ключ-значение" для переменных окружения. Пример:
```env
DB_CONNECTION="Host=localhost;Port=PORT;Database=DBNAME;Username=USER;Password=PASSWORD;"
```

```C#
DotNetEnv.Env.Load();
var connectionString = Environment.GetEnvironmentVariable("DB_CONNECTION");
```
**`Microsoft.Extensions.Configuration:`** Встроенная библиотека в ASP.NET Core.

### Подключение обработчиков запросов.

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

### Методы контроллера для обработки запросов.

```C#
[Route("v1/todo")]
[ApiController]
public class TodoItemsController : ControllerBase  
    { 
        // GET  
        [HttpGet("{id}")]  
        public async Task<T> GetItem(params) {}    
```

### HttpContext
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

### Обработка тела запроса.

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
	    .AsNoTracking()  
        .Skip((page - 1) * perPage)  
        .Take(perPage)  
        .ToListAsync();  
    return OK();  
}
```

### Response

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
    return StatusCode(500, valie: "message");
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

### Entity Framework Core

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
	.AsNoTracking()
    .OrderBy(x => x.Id)
    .Skip((page - 1) * perPage)
    .Take(perPage)
    .ToListAsync();
}
```

### Подключение к базе

   ```csharp
using Microsoft.EntityFrameworkCore;

connection = "Host=localhost;Port=PORT;Database=DBNAME;Username=USER;Password=PASSWORD;"
builder.Services.AddDbContext<ToDoContext>(options => options.UseNpgsql(connection));
   ```

**Использование контекста в коде:**
   Используйте контекст базы данных в коде для выполнения операций с базой данных.

   ```csharp
   using (var context = new TodoDbContext())
   {
       var user = new Todo { Text = "John Doe" };
       context.ToDoItems.Add(todo);
       context.SaveChanges();
   }
   ```

### DTO
DTO (Data Transfer Object) представляет собой объект, который используется для передачи данных между различными компонентами вашего приложения, особенно между серверным и клиентским кодом. 

#### Как использовать DTO в серверных приложениях на C#:

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

   Пример с использованием AutoMapper:
   ```csharp
builder.Services.AddAutoMapper(typeof(AutoMapperProfile));

CreateMap<TodoItem, TodoItemDTO>();

var responseData = _mapper.Map<GetTodoDto>(todoItems);
   ```

### Работа с JSON

#### С использованием System.Text.Json

```csharp
using System.Text.Json;

//Сериализация в JSON:
var item = new { Text = "Read a book" };
string jsonString = JsonSerializer.Serialize(obj);

//Десериализация из JSON:
string jsonString = "{\"Name\":\"John\",\"Age\":30}";
var obj = JsonSerializer.Deserialize<MyClass>(jsonString);
```

#### С использованием Newtonsoft.Json:

```csharp
using Newtonsoft.Json;

//Сериализация в JSON:
var obj = new { Text = "John", Age = 30 };
string jsonString = JsonConvert.SerializeObject(obj);

//Десериализация из JSON:
string jsonString = "{\"Name\":\"John\",\"Age\":30}";
var obj = JsonConvert.DeserializeObject<MyClass>(jsonString);
```
