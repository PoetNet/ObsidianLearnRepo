#Serilog

Запись в логов в базу данных с собственными полями:
https://github.com/b00ted/serilog-sinks-postgresql

```C#
Log.Logger = new LoggerConfiguration()  
    .Enrich.FromLogContext()  
    .WriteTo.PostgreSQL(  
        connectionString: Environment.GetEnvironmentVariable("DB_CONNECTION_STRING"),  
        tableName: "Logs",  
        needAutoCreateTable: true,  
        respectCase: true,  
        columnOptions: new Dictionary<string, ColumnWriterBase>  
        {  
            ["message"] = new RenderedMessageColumnWriter(),  
            ["level"] = new LevelColumnWriter(true, NpgsqlDbType.Varchar),  
            ["raise_date"] = new TimestampColumnWriter(),  
        }  
    )  
    .MinimumLevel.Is(Configuration.GetValue<LogEventLevel>("Logging:LogLevel:Default"))  
    .CreateLogger();  
  
services.AddLogging(loggingBuilder =>  
{  
    loggingBuilder.ClearProviders();  
    loggingBuilder.AddSerilog();  
});
```
