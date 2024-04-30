#TODO 
#### 1. **Как обработать загруженное изображение в ASP.NET Core?**

Чтобы обработать загруженное изображение, вы можете использовать библиотеку `ImageSharp`. Вот пример обработки загруженного изображения:

```csharp
[HttpPost("upload")]
public async Task<IActionResult> UploadImage(IFormFile file)
{
    if (file == null || file.Length == 0)
        return BadRequest("File is not selected or empty");

    using (var stream = new MemoryStream())
    {
        await file.CopyToAsync(stream);
        stream.Seek(0, SeekOrigin.Begin);

        using (var image = Image.Load(stream))
        {
            // Ваша обработка изображения здесь
            // Например, изменение размера или применение фильтров
        }
    }

    return Ok("Image uploaded and processed successfully");
}
```


4. `using (var stream = new MemoryStream()) { ... }`: Создается поток (`MemoryStream`), который будет использоваться для чтения содержимого загруженного файла.

5. `await file.CopyToAsync(stream);`: Загруженный файл `file` копируется в поток `stream`. Это асинхронная операция, которая ожидает, пока весь файл будет скопирован в поток.

6. `stream.Seek(0, SeekOrigin.Begin);`: После копирования файла в поток мы перемещаем указатель потока в начало, чтобы можно было прочитать данные из потока с начала.

8. `// Ваша обработка изображения здесь`: Здесь вы можете добавить свой код для обработки изображения. Например, изменение размера или применение фильтров


#### 2. **Как сохранить файл на диск?**

Чтобы сохранить файл на диск, вы можете использовать метод `SaveAs`:

```csharp
[HttpPost("upload")]
public async Task<IActionResult> UploadFile(IFormFile file)
{
    if (file == null || file.Length == 0)
        return BadRequest("File is not selected or empty");

    var filePath = "path/to/save/" + file.FileName;

    using (var stream = new FileStream(filePath, FileMode.Create))
    {
        await file.CopyToAsync(stream);
    }

    return Ok("File uploaded successfully");
}
```

#### 3. **Как прочитать файл с диска и отправить в ответе?**

Чтобы прочитать файл с диска и отправить его в ответе, используйте следующий код:

```csharp
[HttpGet("download/{fileName}")]
public IActionResult DownloadFile(string fileName)
{
    var filePath = "path/to/file/" + fileName;

    if (!System.IO.File.Exists(filePath))
        return NotFound();

    var fileBytes = System.IO.File.ReadAllBytes(filePath);
    return File(fileBytes, "application/octet-stream", fileName);
}
```

#### 4. **Что такое JWT? Как в ASP.NET Core работать с JWT?**

JWT (JSON Web Token) - это открытый стандарт (RFC 7519) для создания токенов доступа, содержащих утверждения, которые могут быть проверены и удостоверены. В ASP.NET Core вы можете использовать библиотеку `System.IdentityModel.Tokens.Jwt` для работы с JWT. Вот пример создания и проверки JWT:

```csharp
var tokenHandler = new JwtSecurityTokenHandler();
var key = Encoding.ASCII.GetBytes("your_secret_key");
var tokenDescriptor = new SecurityTokenDescriptor
{
    Subject = new ClaimsIdentity(new Claim[]
    {
        new Claim(ClaimTypes.Name, "username"),
        // Другие утверждения
    }),
    Expires = DateTime.UtcNow.AddHours(1),
    SigningCredentials = new SigningCredentials(new SymmetricSecurityKey(key), SecurityAlgorithms.HmacSha256Signature)
};

var token = tokenHandler.CreateToken(tokenDescriptor);
var tokenString = tokenHandler.WriteToken(token);

// Проверка токена
var tokenValidationParameters = new TokenValidationParameters
{
    ValidateIssuerSigningKey = true,
    IssuerSigningKey = new SymmetricSecurityKey(key),
    ValidateIssuer = false,
    ValidateAudience = false
};

var principal = tokenHandler.ValidateToken(tokenString, tokenValidationParameters, out var validatedToken);
```

#### 5. **Что такое сессия? Как работать с сессиями в ASP.NET Core?**

Сессия - это механизм для сохранения состояния между запросами от одного пользователя к другому. В ASP.NET Core сессии могут быть настроены и использованы следующим образом:

```csharp
// Startup.cs
public void ConfigureServices(IServiceCollection services)
{
    services.AddSession(options =>
    {
        options.Cookie.Name = ".YourApp.Session";
        options.IdleTimeout = TimeSpan.FromMinutes(30);
    });

    services.AddMvc();
}

public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    app.UseSession();
    app.UseMvc();
}
```

Использование сессии в контроллере:

```csharp
public IActionResult Index()
{
    HttpContext.Session.SetString("Key", "Value");
    var value = HttpContext.Session.GetString("Key");
    return View();
}
```

#### 6. **Что такое Middleware? Как в ASP.NET Core создать и подключить собственный Middleware?**

Middleware - это компонент, который добавляется в конвейер обработки запросов ASP.NET Core для выполнения определенных задач. Для создания собственного Middleware вам нужно определить класс с методом `Invoke`, который принимает `HttpContext`, и зарегистрировать его в методе `Configure` в `Startup.cs`. Например:

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext context)
    {
        // Действия перед выполнением запроса
        await _next(context);
        // Действия после выполнения запроса
    }
}

// В методе Configure
public void Configure(IApplicationBuilder app)
{
    app.UseMiddleware<CustomMiddleware>();
}
```

#### 7. **Как сделать переадресацию запроса?**

Для переадресации запроса используйте метод `Redirect`:

```csharp
public IActionResult RedirectExample()
{
    return Redirect("/new-url");
}
```

#### 8. **Как изменить статус серверного ответа?**

Для изменения статуса серверного ответа используйте метод `StatusCode`:

```csharp
public IActionResult StatusCodeExample()
{
    return StatusCode(404, "Not Found");
}
```

#### 9. **Как отправить в ответе данные?**

Для отправки данных в ответе используйте методы `View`, `Json`, `File` и другие возвращаемые типы действий контроллера.

```csharp
public IActionResult ViewExample()
{
    return View();
}

public IActionResult JsonExample()
{
    var data = new { Name = "John", Age = 30 };
    return Json(data);
}

public IActionResult FileExample()
{
    var filePath = "path/to/file.txt";
    return PhysicalFile(filePath, "text/plain");
}
```
