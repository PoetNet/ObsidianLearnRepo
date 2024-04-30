#### 1. Рефлексия в C#: что это, как можно применять? 

```C#
object obj = new MyClass();
Type type = obj.GetType();

Type type = typeof(MyClass);

Type type = Type.GetType("MyClass");
```

#### 2. Как получить поля и методы, используя рефлексию? 

Рефлексия в C# - это механизм, который позволяет программам анализировать свою структуру, типы и поведение во время выполнения. С помощью рефлексии можно получать информацию о типах (классах, интерфейсах, структурах и др.), вызывать методы, создавать экземпляры объектов и многое другое.

```c#
using System.Reflection;

var currentAssembly = Assembly.GetExecutingAssembly();

var myPointType = typeof(MyPoint);
FieldInfo[] fields = myPointType.GetFields();
var method = myPointType.GetMethod("PrintDefaultPoint", BindingFlags.NonPublic | BindingFlags.Static);
var point = Activator.CreateInstance(myPointType, -1, 1);

method?.Invoke(point, null);
method?.Invoke(null, null);


struct MyPoint(int X, int Y)
{
    private static readonly int _defaultX = 0;
    private static readonly int _defaultY = 0;

    private static void PrintDefaultPoint() => Console.WriteLine($"Default: X = {_defaultX}, Y = {_defaultY}");

    public void Print() => Console.WriteLine($"X = {X}, Y = {Y}");
}
```

#### 3. Dependency Injection (DI): что это такое? Как можно использовать?

 Задача:
```C#
public class ConsoleLogger
{
    public void Log(string message) => Console.WriteLine(message);
}

public class MyClass
{
    public void DoSomething()
    {
        // ..логика
        logger.Log("Finished Method");
    }
}
```

Ответ:
```C#
using Microsoft.Extensions.DependencyInjection;

var services = new ServiceCollection();                 // Создание IServiceCollection

services.AddSingleton<ILogger, ConsoleLogger>();        // Регистрация зависимости в IServiceCollection
services.AddTransient<MyClass>();                       // Регистрация класса, зависящего от ILogger

var serviceProvider = services.BuildServiceProvider();  // Создание ServiceProvider

using var scope = serviceProvider.CreateScope();
var myClass = scope.ServiceProvider.GetRequiredService<MyClass>();

myClass.DoSomething();

public interface ILogger
{
    void Log(string message);
}


public class ConsoleLogger : ILogger
{
    public void Log(string message) => Console.WriteLine(message);
}

public class MyClass(ILogger logger)
{
    public void DoSomething()

    {
        // ..логика
        logger.Log("Doing something"); // Использование зависимости
    }
}

```

#### 5. Анонимные типы и методы.

   Анонимный тип  используется чаще всего для создания объектов, содержащих набор свойств, когда точный тип объекта не так важен, как его структура данных.

   ```csharp
   var person = new { Name = "John", Age = 30 }; // <>f__AnonymousType0'2
   ```

Анонимный метод - это метод, который объявляется без имени, но с помощью ключевого слова `delegate`. Он позволяет передать код как параметр в другие методы, такие как методы событий или методы LINQ, без явного определения именованного делегата.

   ```csharp
   Action<int> printNumber = delegate(int num) 
   {
       Console.WriteLine($"Number: {num}");
   };
   ```

#### 6. Что такое делегаты? Как они работают и для чего они нужны?


```C#
Action<string> Greeting = (string name) => Console.WriteLine($"Hello, {name}");
Func<int, int, int> Sum = (int val1, int val2) => val1 + val2;
Predicate<int> isOdd = (int num) => num % 2 == 1;


// Создание экземпляра делегата и привязка к методу DisplayMessage
MyDelegate delegateInstance = new MyDelegate(DisplayMessage);

// Вызов метода через делегат
delegateInstance("Привет, мир!");

Console.ReadLine();

// Определение делегата
delegate void MyDelegate(string message);

// Метод, который будет вызываться через делегат
static void DisplayMessage(string message)
	=> Console.WriteLine("Сообщение: " + message);
```


#### 10. Интерфейс IDisposable: как использовать?

```C#
public class SomeClass : IDisposable

{
    private bool _disposed = false;
    
    public void Dispose() // реализация интерфейса IDisposable.
    {
        Dispose(true); // освобождаем неуправляемые ресурсы
        GC.SuppressFinalize(this); // подавляем финализацию
    }

    private void Dispose(bool disposing)

    {
        if (_disposed) return;
        if (disposing)
        {
            // Освобождаем управляемые ресурсы
        }
        // освобождаем неуправляемые объекты
        _disposed = true;
    }
    
    ~SomeClass() // Деструктор
    {
        Dispose(false);
    }
}
```

#### 11. Сколько значений возвращает функция или метод в C#? Как можно вернуть несколько значений?

 **Использование кортежей (Tuple)**

```C#
public (int, string) GetValues() 
{     
	int intValue = 10;
	string stringValue = "Hello";     
	return (intValue, stringValue); 
} 
 
(int value1, string value2) = GetValues(); 
Console.WriteLine($"Value 1: {value1}, Value 2: {value2}");`
```

**Использование out-параметров**.

**Использование класса или структуры**.

#### 14. Работа с датами: Как получить текущую дату? Как получить UNIX TIMESTAMP текущего времени? Как осуществлять форматирование даты и времени? 

Как получить UNIX TIMESTAMP текущего времени?

```C#
var currentDateTime = DateTimeOffset.UtcNow; 
long unixTimestamp = currentDateTimeOffset.ToUnixTimeSeconds();
```
---
Как осуществлять форматирование даты и времени?

```C#
// Формат: 2024-04-01 10:35:17
// Формат: 10:38 | 01 апреля


var currentDate = DateTime.Now; 

string formattedDate1 = currentDate.ToString("yyyy-MM-dd HH:mm:ss");
string formattedDate2 = currentDate.ToString("HH:mm | dd MMMM");
```
---
Написать метод, который будет вычислять возраст человека на момент указанной даты.

```C#
public int CalculateAge(DateTime birthDate, DateTime onDate)
{
    int age = onDate.Year - birthDate.Year;
    if (onDate.Month < birthDate.Month || (onDate.Month == birthDate.Month && onDate.Day < birthDate.Day))
    {
        age--;
    }
    return age;
}
```


#### 16. Интерфейсы IComparer и IComparable. Зачем нужны и как использовать?

```C#
struct MyPoint : IComparable<MyPoint>
{
    public int X { get; set; }
    public int Y { get; set; }

    public int CompareTo(MyPoint other)
    {
        double distance1 = Math.Sqrt(X * X + Y * Y);
        double distance2 = Math.Sqrt(other.X * other.X + other.Y * other.Y);

        return distance1.CompareTo(distance2);
    }

    public void Print() => Console.WriteLine($"X = {X}, Y = {Y}");
}

class PointSumComparer : IComparer<MyPoint>
{
    public int Compare(MyPoint point1, MyPoint point2)
    {
        int sum1 = point1.X + point1.Y;
        int sum2 = point2.X + point2.Y;

        return sum1.CompareTo(sum2);
    }
}
```


#### 18. Logging

```C#
 ============================== Логирование ==============================

    Базовые уровни логирования
 ● TRACE (трассировка)      ● WARN (предупреждение)

 ● DEBUG (отладка)          ● ERROR (ошибка)

 ● INFO (информация)        ● FATAL (критическая ошибка)    
---------------------------
    Инструменты логирования
    
 ● log4net                  ● Microsoft.Extensions.Logging: ASP.NET Core.

 ● NLog                      

 ● Serilog                        
---------------------------

    Каналы записи в лог
 ● File (Файл)              ● Event Log (Журнал событий)              

 ● Console (Консоль):       ● Remote Server (Удаленный сервер)

 ● Database (База данных)        

    =================== LINQ (Language Integrated Query) ===================

 ● LINQ to Objects          ● LINQ to XML
 ● LINQ to Entities         ● Parallel LINQ  
```


#### 22. Что такое LINQ? Как использовать LINQ для реализации методов Map, Filter, Reduce?

   ```csharp
   var originalList = new List<int> { 1, 2, 3, 4, 5 };
   
   var mappedList = originalList.Select(x => x * 2).ToList();
   var filteredList = originalList.Where(x => x % 2 == 0).ToList();
   var sum = originalList.Aggregate((acc, x) => acc + x);
   ```

#### 27. Чем различаются Task и Thread? Async / await: как работает? Как использовать?
```C#
Task task1 = Task.Run(() => CalculateResult1());
Task task2 = Task.Run(() => CalculateResult2());
Task task3 = Task.Run(() => CalculateResult3());

Console.WriteLine("Завершение основного потока.");

Task CalculateResult1()
{
    Thread.Sleep( 1000 );
    Console.WriteLine("Метод 1 завершил работу.");
    return Task.CompletedTask;
}

Task CalculateResult2()
{
    Thread.Sleep(2000);
    Console.WriteLine("Метод 2 завершил работу.");
    return Task.CompletedTask;
}

Task CalculateResult3()
{
    Thread.Sleep(3000);
    Console.WriteLine("Метод 3 завершил работу.");
    return Task.CompletedTask;
}
```

await Task.WhenAll(task1, task2, task3);

#### All

```C#
// =================== Многопоточное программирование ===================

var rndGen = new Random();
var thread1 = new Thread(DoWork);
var thread2 = new Thread(() => DoParametrizedWork(1000));

thread1.Start();
thread2.Start();

thread2.Join(); // ждем выполнение потока


//TASK

Task task1 = new Task(() => Console.WriteLine("Hello Task!"));
task.Start();
task.Wait();

Task task2 = Task.Factory.StartNew(() => Console.WriteLine("Another task"));



// БЛОКИРОВКА ПОТОКА

int x = 0;

object locker = new();  // объект-заглушка

// запускаем пять потоков
for (int i = 1; i < 6; i++)
{
    Thread myThread = new(Print);
    myThread.Name = $"Поток {i}";
    myThread.Start();
}

void Print()
{
    lock (locker)
    {
        x = 1;
        for (int i = 1; i < 6; i++)
        {
            Console.WriteLine($"{Thread.CurrentThread.Name}: {x}");
            x++;
        }
    }
}

// Теперь MUTEX

int x = 0;
Mutex mutexObj = new();

// запускаем пять потоков

for (int i = 1; i < 6; i++)
{
    Thread myThread = new(Print);
    myThread.Name = $"Поток {i}";
    myThread.Start();
}

void Print()
{
    mutexObj.WaitOne();     // приостанавливаем поток до получения мьютекса
    x = 1;
    for (int i = 1; i < 6; i++)
    {
        Console.WriteLine($"{Thread.CurrentThread.Name}: {x}");
        x++;
        Thread.Sleep(100);
    }
    mutexObj.ReleaseMutex();    // освобождаем мьютекс
} 
*/
```
