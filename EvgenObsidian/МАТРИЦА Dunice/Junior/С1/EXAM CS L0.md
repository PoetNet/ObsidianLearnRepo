#TODO
### Работа с файлами
```C#
// Открытие файла с диска
string filePath = "example.txt";
using (StreamReader reader = new StreamReader(filePath))
{
    string content = reader.ReadToEnd();
}

// Запись файла на диск
string contentToWrite = "Пример текста для записи в файл.";
string filePathToWrite = "example2.txt";
using (StreamWriter sw = new StreamWriter(filePathToWrite))
{
    sw.Write(contentToWrite);
}

// Чтение содержимого директории
string directoryPath = @"C:\";
string[] files = Directory.GetFiles(directoryPath);

foreach (string file in files)
    Console.WriteLine(file);

// Проверка существования файла или директории
string pathToCheck = "example.txt";
if (File.Exists(pathToCheck))
    Console.WriteLine("Файл существует.");
else
    Console.WriteLine("Файл не существует.");

string directoryToCheck = @"C:\ExampleDirectory";
if (Directory.Exists(directoryToCheck))
{
    Console.WriteLine("Директория существует.");
}
else
{
    Console.WriteLine("Директория не существует.");
}

// Создание директории
string directoryToCreate = @"C:\NewDirectory";
Directory.CreateDirectory(directoryToCreate);
Console.WriteLine("Директория успешно создана.");

// Удаление файла
string fileToDelete = "example2.txt";
File.Delete(fileToDelete);
Console.WriteLine("Файл успешно удалён.");

// Удаление директории (с файлами)
string directoryToDelete = @"C:\NewDirectory";
Directory.Delete(directoryToDelete, true);
Console.WriteLine("Директория успешно удалена вместе с содержимым.");
```

### SQL

#### JOIN
```PostgreSQL
SELECT b.name AS band_name, a.name AS album_name
FROM bands AS b
JOIN albums AS a ON b.band_id = a.band_id;
```
#### DISTINCT
```PostgreSQL
SELECT DISTINCT year
FROM albums;
--Этот запрос выводит список уникальных годов выпуска альбомов.
```
#### GROUP BY
Оператор `GROUP BY` используется для объединения строк с одинаковыми значениями в одну строку. Обычно он применяется с агрегатными функциями (например, `COUNT`, `SUM`, `AVG`) для выполнения операций на группах строк.
```PostgreSQL
SELECT year, COUNT(*) AS album_count
FROM albums
GROUP BY year;

--Этот запрос группирует данные по году выпуска альбомов и выводит количество альбомов для каждого года.
```
#### HAVING (фильтрация сгруппированных данных)
Оператор `HAVING` применяется после `GROUP BY` и позволяет фильтровать группы строк на основе условий.
```PostgreSQL
SELECT year, COUNT(*) AS album_count
FROM albums
GROUP BY year
HAVING COUNT(*) > 1;

--Этот запрос группирует данные по году выпуска альбомов и выводит только те года, для которых количество альбомов больше одного.
```
#### WITH (CTE - Общий Таблицы Выражение)
```PostgreSQL
WITH popular_bands AS (
    SELECT band_id, COUNT(*) AS album_count
    FROM albums
    GROUP BY band_id
    HAVING COUNT(*) > 2
)
SELECT b.name AS band_name, pb.album_count
FROM bands b
JOIN popular_bands pb ON b.band_id = pb.band_id;

--Этот запрос создает общее таблицы выражение `popular_bands`, которое находит группы с более чем двумя альбомами, а затем выводит их названия и количество альбомов.
```

### DELETE TRUNCATE
1. **DELETE**:
    - Оператор `DELETE` используется для удаления строк из таблицы на основе заданного условия.
    - `DELETE` можно использовать для удаления определенных строк с помощью условия `WHERE`.
    - При использовании `DELETE` строки удаляются одна за другой, что может привести к повышенной нагрузке на сервер и замедлению процесса удаления, особенно при большом количестве строк.

```SQL
DELETE FROM table_name WHERE condition;
```

2. **TRUNCATE**:
    - Оператор `TRUNCATE` также используется для удаления данных из таблицы, но он удаляет все строки из таблицы без использования условий.
    - `TRUNCATE` работает намного быстрее, чем `DELETE`, потому что он удаляет все строки целиком и не сохраняет логи изменений.
    - В отличие от `DELETE`, `TRUNCATE` не может быть отменен с помощью оператора `ROLLBACK`.
    - После использования `TRUNCATE` счетчик идентификаторов строки сбрасывается на начальное значение (обычно 1).

```PostgreSQL
TRUNCATE TABLE table_name;
```

### Что такое Constraints, какие бывают?
В контексте PostgreSQL "Constraints" (ограничения) - это правила, которые определяются для столбцов таблицы и определяют, какие данные могут быть вставлены, обновлены или удалены из таблицы. Они гарантируют целостность данных и обеспечивают согласованность базы данных. Вот некоторые типы ограничений, которые могут использоваться в PostgreSQL:

1. **PRIMARY KEY Constraint (Ограничение первичного ключа)**: Гарантирует уникальность и идентификацию каждой строки в таблице. Первичный ключ также автоматически создает индекс для ускорения поиска.

   ```Postgresql
   CREATE TABLE table_name (
       column1 datatype PRIMARY KEY,
       column2 datatype,
       ...
   );
   ```

2. **UNIQUE Constraint (Уникальное ограничение)**: Гарантирует уникальность значений в столбце или группе столбцов.

   ```Postgresql
   CREATE TABLE table_name (
       column1 datatype UNIQUE,
       column2 datatype,
       ...
   );
   ```

3. **FOREIGN KEY Constraint (Внешний ключ)**: Обеспечивает ссылочную целостность между двумя таблицами. Значения в столбце или столбцах указывают на значения в другой таблице.

   ```Postgresql
   CREATE TABLE table_name (
       column1 datatype,
       column2 datatype,
       ...
       CONSTRAINT fk_name FOREIGN KEY (column1) REFERENCES other_table(other_column)
   );
   ```

4. **CHECK Constraint (Ограничение проверки)**: Позволяет определить условие, которое данные должны удовлетворять при вставке или обновлении.

   ```Postgresql
   CREATE TABLE table_name (
       column1 datatype,
       column2 datatype,
       ...
       CONSTRAINT check_name CHECK (condition)
   );
   ```

5. **NOT NULL Constraint (Ограничение NOT NULL)**: Гарантирует, что столбец не может содержать NULL-значения.

   ```Postgresql
   CREATE TABLE table_name (
       column1 datatype NOT NULL,
       column2 datatype,
       ...
   );
   ```

Это основные ограничения, которые можно использовать в PostgreSQL. Они позволяют определять правила, которые данные в таблицах должны соблюдать, чтобы обеспечить целостность и согласованность данных в базе данных.

### Что такое бизнес - логика в контексте разработки веб-приложений?
Бизнес-логика в контексте разработки веб-приложений относится к части программного кода, которая определяет, как приложение взаимодействует с данными, обрабатывает их и принимает решения на основе бизнес-правил и требований.

Веб-приложения, как правило, взаимодействуют с базами данных, обрабатывают данные, полученные от пользователей, и выполняют операции в соответствии с бизнес-правилами. Бизнес-логика реализует эти правила и процессы, которые определяют, как приложение должно работать, чтобы удовлетворить потребности бизнеса и пользователей.

Примеры бизнес-логики в веб-приложениях могут включать:

1. **Проверка подлинности и авторизация пользователей**
    
2. **Валидация данных**
    
3. **Обработка бизнес-логики
    
4. **Управление состоянием**: Определение того, как приложение должно реагировать на различные ситуации и события, например, обработка ошибок или уведомлений.
    
5. **Взаимодействие с внешними системами**: Интеграция с внешними API, сервисами или другими приложениями для получения или отправки данных.
    
6. **Безопасность**: Применение методов шифрования, аутентификации и авторизации для обеспечения безопасности данных и защиты приложения от угроз.

### Виды приложений ASP.NET.
1. Minimal API  
2. MVC (Model-View-Controller)  
3. Web API  
4. Blazor  
5. Razor Pages  
6. SignalR  
7. WebForms  
8. Single Page Applications (SPA)  
9. Web Services  
10. RESTful Services

### Отличия Minimal API от MVC
[[Отличия Minimal API от MVC, их плюсы и минусы]]

### Как заменить стандартный порт, на котором запускается приложение ASP.NET?

#### Используя опцию командной строки --urls:
```sh
dotnet run --urls=http://localhost:5001
```

#### Изменение в файле appsettings.json:
```JSON
{
  "Urls": "http://localhost:5001"
}
```
#### Настройка порта в Program.cs:
```C#
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseUrls("http://localhost:5001")
        .UseStartup<Startup>();

```
#### Или в методе ConfigureServices вашего класса Startup.cs
```C#
public void ConfigureServices(IServiceCollection services)
{
  services.Configure<Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>(options =>
    {
        options.Listen(IPAddress.Any, 5001);
    });
}
```

### Как работать с sensitive конфигурацией в ASP.NET?
1. **User Secrets**:
   В ASP.NET Core есть инструмент под названием User Secrets, который позволяет хранить конфиденциальную информацию локально на вашем компьютере вне репозитория кода.

2. **Environment Variables**:
   Другой способ работы с конфиденциальной информацией - использование переменных среды. 

3. **Secret Manager**:
   Secret Manager - это инструмент командной строки, который позволяет хранить конфиденциальную информацию вне кода приложения. Это похоже на User Secrets, но работает на уровне проекта, а не на уровне пользователя.

4. **Azure Key Vault**:
   Если вы развертываете ваше приложение в Azure, то можете использовать Azure Key Vault для хранения конфиденциальной информации. Azure Key Vault обеспечивает безопасное хранение и управление секретами, ключами и сертификатами.

5. **Шифрование и хеширование**:
   Если вы храните конфиденциальную информацию в базе данных или файлах, хорошей практикой является шифрование такой информации перед хранением и дешифрование при необходимости.

### Как обработать загруженный файл в ASP.NET? 
В ASP.NET вы можете обработать загруженное изображение с использованием библиотеки .NET Core, такой как `System.Drawing.Common`, `ImageSharp`, `Magick.NET` или других сторонних библиотек.
```C#
using Microsoft.AspNetCore.Mvc;

[Route("api/[controller]")]
[ApiController]
public class ImageController(ImageProcessor imageProcessor) : ControllerBase
{
    [HttpPost("resize")]
    public IActionResult ResizeImage([FromForm] IFormFile file, int width, int height)
    {
        using (MemoryStream memoryStream = new MemoryStream())
        {
            file.CopyTo(memoryStream);
            byte[] imageData = memoryStream.ToArray();
            byte[] resizedImageData = _imageProcessor.ResizeImage(imageData, width, height);
            return File(resizedImageData, "image/png"); // возвращаем измененное изображение
        }
    }
}


// Вариант exel?
var fileName = Guid.NewGuid().ToString() + Path.GetExtension(file?.FileName);  
string contentRootPath = _env.ContentRootPath;  
var filePath = Path.Combine(contentRootPath, ""images", fileName);  
  
using (var fileStream = new FileStream(filePath, FileMode.Create))  
{  
await file!.CopyToAsync(fileStream);  
}
```

### Как прочитать файл с диска и отправить в ответе?
```C#
var fileInfo = _fileProvider.GetFileInfo(fileName);  
var stream = fileInfo.CreateReadStream();  
return Results.File(fileInfo.PhysicalPath!);

//

[Route("api/[controller]")]
[ApiController]
public class FileController : ControllerBase
{
    [HttpGet("{filename}")]
	public async Task<IActionResult> Get(string filename)
	{
	    var filePath = Path.Combine(hostingEnvironment.WebRootPath, "Files", filename);
            
	    if (!System.IO.File.Exists(filePath))
	    {
	        return NotFound();   
	    }

	    return File(filePath, "application/octet-stream");
	}
}
```

### MiddleWare
### Создание собственного обработчика:

#### Кастомный класс обработчика
```C#
public class CustomMiddleware(RequestDelegate next)
{
    public async Task InvokeAsync(HttpContext context)
    {
        var token = context.Request.Query["token"];
        if (token!="Very_Secret_Token")
        {
            context.Response.StatusCode = 403;
            await context.Response.WriteAsync("Token is invalid");
        }
        else
            await _next.Invoke(context);
    }
}
```

#### Файл Startup.cs
```C#
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.UseMiddleware<TokenMiddleware>();
 
        app.Run(async (context) =>
        {
            // DoWork
        });
    }
}
```

### Возвращение результата с определенным статус-кодом и данными:
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
