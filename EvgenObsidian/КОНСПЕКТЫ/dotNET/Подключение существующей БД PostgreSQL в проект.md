#EFCore #PostgreSQL

Документация : https://learn.microsoft.com/en-us/ef/core/cli/dotnet
Metanit: https://metanit.com/sharp/entityframeworkcore/1.3.php

1. Добавить через Nuget пакет:

```C#
Npgsql.EntityFrameworkCore.PostgreSQL
```

2. Подключить БД через строку подключения:

```C#
DotNetEnv.Env.Load();
var connectionString = Environment.GetEnvironmentVariable("DB_CONNECTION_STRING");
string connection = builder.Configuration.GetConnectionString(connectionString!)!;

builder.Services.AddDbContext<ToDoContext>(options => options.UseNpgsql(connection));
```

3. Установить в ОС .NET Core CLI:

```Shell
dotnet tool install --global dotnet-ef
dotnet tool update  --global dotnet-ef
```

4. Добавить в проект пакет `Microsoft.EntityFrameworkCore.Design`:

```Shell
dotnet add package Microsoft.EntityFrameworkCore.Design
```

5. Создать все необходимые классы по базе данных (в директории проекта):

```Shell
dotnet ef dbcontext Scaffold "Host=localhost;Port=5432;Database=ToDo;Username=postgres;Password=Dunice;" Npgsql.EntityFrameworkCore.PostgreSQL
```
