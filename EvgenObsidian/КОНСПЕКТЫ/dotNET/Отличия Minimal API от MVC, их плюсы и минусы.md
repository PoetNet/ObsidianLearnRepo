#MinimalAPI #WebAPI #MVC 

Одним из недостатков фреймворка Web API (например, контроллеров) является то, что для небольших API (или микросервисов) требуется определенная церемония. Например, контроллеры — это классы, которые представляют один или несколько возможных вызовов API: 

```C#
[Route("api/[Controller]")]
[Authorize(AuthenticationSchemes = JwtBearerDefaults.AuthenticationScheme)]
public class OrdersController(
        IAppRepository repository,
        ILogger<OrdersController> logger,
        IMapper mapper,
        UserManager<StoreUser> userManager)
         : Controller
{
    [HttpGet]
    public IActionResult Get(bool includeItems = true)
    {
        try
        {
            var username = User.Identity.Name;
            var results = repository
                .GetOrdersByUser(username, includeItems);

            return Ok(_mapper
                .Map<IEnumerable<OrderViewModel>>(results));
        }
        catch (Exception ex)
        {
            logger.LogError($"Failed : {ex}" );
            return BadRequest($"Failed");
        }
    }
...
```

Этот код типичен для контроллеров Web API. Но значит ли это, что он плох? Нет. Для больших API и тех, которые имеют расширенные потребности (например, полноценная аутентификация, авторизация и версионирование), такая структура работает отлично. Но для некоторых проектов действительно необходим более простой способ создания API. 

### **Что такое Minimal API?**

Основная идея Minimal API заключается в том, чтобы устранить контроллеры при создании простых API. Это означает определение лямбда-выражений для отдельных вызовов API. Например:

```C#
app.MapGet("/", () => "Hello World!");
```

Этот вызов указывает маршрут (например, "/") и обратный вызов для выполнения после совпадения запроса, соответствующего маршруту и глаголу. Метод `MapGet` предназначен для маппинга HTTP GET с функцией обратного вызова. Большая часть этого волшебства заключается в том, что происходит определение типа. Когда мы возвращаем строку (как в этом примере), он оборачивает ее в возвращаемый результат 200 (OK, например).

Данные методы маппинга являются открытыми. Это методы расширения в интерфейсе `IEndpointRouteBuilder`. Интерфейс раскрывается классом `WebApplication`, который используется для создания нового приложения Web-сервера в .NET 6. 

> Основная идея Minimal APIs заключается в том, чтобы убрать некоторые церемонии создания простых API.

### Новый способ использования Startup

Многое уже было написано о желании убрать шаблонный код из процедуры начального запуска в C# в целом. С этой целью Microsoft добавила в C# 10 так называемые "утверждения верхнего уровня". Это означает, что `program.cs`, с помощью которого вы запускаете свои веб-приложения, не нуждается в `void Main()` для загрузки приложения. До появления C# 10 запуск приложения выглядел примерно так:

```C#
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }
    
    public static IHostBuilder
        CreateHostBuilder(string[] args) => Host.CreateDefaultBuilder(args)
			.ConfigureWebHostDefaults(webBuilder =>
	        {
	            webBuilder.UseStartup<Startup>();
	        });
}
```

Необходимость в классе и методе `void Main`, который выполняет загрузку хоста для запуска сервера, — это то, как мы уже несколько лет пишем ASP.NET в  .NET Core. С помощью операторов верхнего уровня этот шаблон кода был упрощен, как показано ниже:

```C#
var builder = WebApplication.CreateBuilder(args);

// Setup Services
var app = builder.Build();

// Add Middleware

// Start the Server
app.Run();
```

Вместо класса `Startup` с местами для настройки сервисов и `Middleware`, все делается в данной очень простой программе верхнего уровня. Какое отношение это имеет к `Minimal API`? Приложение, создаваемое объектом builder, поддерживает интерфейс `IEndpointRouteBuilder`. Так что в нашем случае настройка API — это просто `Middleware`:

```C#
var builder = WebApplication.CreateBuilder(args);

// Setup Services
var app = builder.Build();

// Map APIs
app.MapGet("/", () => "Hello World!");

// Start the Server
app.Run();
```

### Маршрутизация

Первое, что бросается в глаза — паттерн для маппинга вызовов API очень похож на паттерн соответствия контроллеров MVC. Это означает, что Minimal API очень похожи на методы контроллера. Например:

```C#
app.MapGet("/api/clients", () => new Client()
{
    Id = 1,
    Name = "Client 1"
});
app.MapGet("/api/clients/{id:int}", (int id) => new Client()
{
    Id = id,
    Name = "Client " + id
});
```

Простые пути, такие как `/api/clients`, указывают на простые пути URI, поскольку использование синтаксиса параметров (даже с ограничениями) продолжает работать. Обратный вызов может принимать ID, маппированный из URI, как и контроллеры MVC. Одна вещь, на которую следует обратить внимание в лямбда-выражении, заключается в том, что типы параметров инференцируются (как и в большинстве случаев в C#). Это означает, что поскольку вы используете URL-параметр  (например, `id`), вам нужно ввести первый параметр. Если вы его не ввели, то в лямбда-выражении будет сделана попытка угадать тип:

```C#
app.MapGet("/api/clients/{id:int}", (id) => new Client()
{
    Id = id, // Doesn't Work
    Name = "Client " + id
});
```

Это не работает, так как без подсказки типа первый параметр лямбда-выражения считается экземпляром `HttpContext`. Так происходит потому, что на самом низком уровне можно управлять собственным ответом на любой запрос с помощью объекта контекста. Но в большинстве случаев используются параметры лямбда-выражения для помощи в маппинге объектов и параметров.

### Использование сервисов

В большинстве случаев для выполнения вызовов хочется воспользоваться обычными сервисами. Это подводит к вопросу о том, как использовать сервисы в Minimal API. 
Можно просто использовать объект builder для доступа к службам, например, так:

```C#
var builder = WebApplication.CreateBuilder(args);

// Register services
builder.Services.AddDbContext<AppContext>();
builder.Services.AddTransient<IAppRepository, AppRepository>();

var app = builder.Build();
```

Можно использовать объект `Services` в builder приложения для добавления любых необходимых сервисов. Чтобы использовать эти сервисы, можно просто добавить их в параметры лямбда-выражения:

```C#
app.MapGet("/clients", async (IAppRepository repo) => {
    return await repo.GetClientsAsync();
});
```

При добавлении требуемого типа он будет введен в лямбда-выражение во время его выполнения. Это отличается от API на основе контроллеров тем, что зависимости обычно определяются на уровне классов. Эти инжектированные сервисы не меняют того, как службы обрабатываются сервисным уровнем (т.е. Minimal API по-прежнему создает область действия для ограниченных служб). При использовании параметров URI можно просто добавить необходимые сервисы к другим параметрам: 

```C#
app.MapGet("/clients/{id:int}", async (int id, IAppRepository repo) => {
    return await repo.GetClientAsync(id);
});
```

Для этого потребуется отдельно продумать сервисы, необходимые для каждого вызова API. Но это также обеспечивает гибкость в использовании сервисов на уровне API.

### Глаголы

До сих пор рассматривались только HTTP GET API. Существуют методы для различных типов глаголов. К ним относятся:

- MapPost

- MapPut

- MapDelete


Эти методы работают идентично `MapGet`. Например, рассмотрим вызов для POST нового клиента:

```C#
app.MapPost("/clients", async (Client model, IAppRepository repo) =>
{
    // ...
});
```

В данном случае модели не нужно использовать атрибуты для указания `FromBody`. Она определяет тип, если форма соответствует запрашиваемому типу. Вы можете смешивать и сочетать все, что вам может понадобиться (как показано в `MapPut`):

```C#
app.MapPut("/clients/{id}", async (int id, ClientModel model, 
    IAppRepository repo) =>
{
    // ...
});
```

Для других глаголов вам нужно выполнять их маппинг с помощью MapMethods:

```C#
app.MapMethods("/clients", new [] { "PATCH" }, 
    async (IAppRepository repo) => {return await repo.GetClientsAsync();
});
```

Метод `MapMethods` использует не только путь, но и список глаголов, которые нужно принять. В данном выполняется это лямбда-выражение при получении глагола PATCH. 
### Использование кодов состояния HTTP

До сих пор в этих примерах не рассматривалась обработка различных результатов действий API. В большинстве API, которые я пишу, невозможно предположить, что действие будет успешным, а выбрасывать исключения — не тот способ, который подходит. Для этого необходимо контролировать возвращаемые коды состояния. Этим занимается статический класс `Results`. Можно просто обернуть свой результат вызовом `Results` и кодом состояния:

```C#
app.MapGet("/clients", async (IAppRepository repo) => {
 return Results.Ok(await repo.GetClientsAsync());
});
```

Results поддерживает большинство кодов состояния, которые могут понадобятся, например:

- **Results.Ok:** 200

- **Results.Created:** 201

- **Results.BadRequest:** 400

- **Results.Unauthorized:** 401

- **Results.Forbid:** 403

- **Results.NotFound**: 404


В типовом сценарии можно использовать несколько из них:

```C#
app.MapGet("/clients/{id:int}", async (int id, IAppRepository repo) => {
    try {
        var client = await repo.GetClientAsync(id);
        if (client == null)
        {
            return Results.NotFound();
        }
        return Results.Ok(client);
    }
    catch (Exception ex)
    {
        return Results.BadRequest("Failed");
    }
});
```

Если необходимо передать делегат классам `MapXXX`, можно просто заставить их возвращать `IResult` для запроса кода состояния:

```C#
app.MapGet("/clients/{id:int}", HandleGet);
async Task<IResult> HandleGet(int id, IAppRepository repo)
{
    try
    {
        var client = await repo.GetClientAsync(id);
        if (client == null) return Results.NotFound();
        return Results.Ok(client);
    }
    catch (Exception)
    {
        return Results.BadRequest("Failed");
    }
}
```

Т.к. в этом примере применяется `async`,  нужно обернуть `IResult` объектом `Task`. В результате возвращается экземпляр `IResult`. Хотя Minimal API предназначены для того, чтобы быть маленькими и простыми, с прагматической точки зрения API меньше зависит от того, как они инстанцируются, и больше от логики внутри них. И Minimal API, и API на основе контроллеров работают по сути одинаково.

## Обеспечение безопасности Minimal API

Хотя Minimal API работают с Middleware аутентификации и авторизации, все равно может понадобится способ указать на уровне API, как должна работать безопасность. При работе с API на основе контроллеров, можно использовать атрибут `[Authorize]` для указания способов защиты API, но если нет контроллеров, остается указать их на уровне API. Это делается вызовом методов на сгенерированных вызовах API. Пример требования авторизации:

```C#
app.MapPost("/clients", async (ClientModel model, IAppRepository repo) =>
{
    // ...
}).RequireAuthorization();
```

Этот вызов `RequireAuthorization` равносилен использованию фильтра `Authorize` в контроллерах (например, можно указать, какова схема аутентификации или другие необходимые свойства). Допустим, необходимо требовать аутентификацию для всех вызовов:

```C#
builder.Services.AddAuthorization(cfg => 
{
	cfg.FallbackPolicy = new AuthorizationPolicyBuilder()
        .RequireAuthenticatedUser()
        .Build();
});
```

Тогда не придется добавлять `RequireAuthentication` для каждого API, но можно переопределить это значение по умолчанию, разрешив анонимность для других вызовов:

```C#
app.MapGet("/clients", async (IAppRepository repo) =>
{
    return Results.Ok(await repo.GetClientsAsync());
}).AllowAnonymous();
```

Таким образом, можно смешивать и сочетать аутентификацию и авторизацию по своему усмотрению.

### Использование Minimal API без функций верхнего уровня

Большая часть "магии" Minimal API исходит от интерфейса `IEndpointRouteBuilder`. Его поддерживает не только класс `WebApplication`, он также используется в традиционном классе Startup. При вызове `UseEndpoints`, делегат, указанный там, передает `IEndpointRouteBuilder`, что означает — можно просто вызвать `MapGet`:

```C#
public void Configure(IApplicationBuilder app,
IWebHostEnvironment env)
{
if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    app.UseRouting();
    app.UseAuthorization();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapGet("/clients", async (IAppRepository repo) =>
        {
            return Results.Ok(await repo.GetClientsAsync());
        }).AllowAnonymous();
    });
}
```

