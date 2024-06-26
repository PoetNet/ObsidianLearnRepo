### Ключевое слово *var*

В C# поддерживается возможность определения типа используемых в методе локальных переменных по типу используемого при их инициализации выражения:

```C#
private static void ImplicitlyTypedLocalVariables() 
{ 
	var name = "Evgen"; 
	ShowVariableType(name); // Вывод: System.String 
	
	// var n = null; // Ошибка 
	var x = (string)null;   // Допустимо, хотя и бесполезно 
	ShowVariableType(x);    // Вывод: System.String 
	
	var numbers = new int[] { 1, 2, 3, 4 }; 
	ShowVariableType(numbers); // Вывод: System.Int32[] 
	
	// Меньше символов при вводе сложных типов 
	var collection = new Dictionary<string, float>() { { "Grant", 4.0f } }; 
	// Вывод: System.Collections.Generic.Dictionary`2[System.String,System.Single] 
	
	ShowVariableType(collection);
	 
	foreach (var item in collection) 
	{ 
		// Вывод: System.Collections.Generic.KeyValuePair`2 
		// [System.String,System.Single] 
		ShowVariableType(item); 
	} 
} 

private static void ShowVariableType<T>(T type) => Console.WriteLine(typeof(T));
```

Первая строка кода метода `ImplicitlyTypedLocalVariables` вводит новую локальную переменную при помощи ключевого слова `var`. Чтобы определить ее тип, компилятор смотрит на тип выражения с правой стороны от оператора присваивания (=). Так как "Jeff" — это строка, компилятор присваивает переменной `name` тип `string`. Чтобы доказать, что компилятор правильно определяет тип, напишем универсальный метод `ShowVariableType`. Он определяет тип своего аргумента и выводит его на консоль.

Вторая операция присваивания (закомментированная) в методе `ImplicitlyTypedLocalVariables` во время компиляции привела бы к ошибке : 
	`Error CS0815: Cannot assign to an implicitly-typed local variable` 
Дело в том, что значение `null` неявно приводится к любому ссылочному типу или значимому типу, допускающему значение `null`. Соответственно, компилятор не в состоянии однозначно определить его тип. Однако в третьей операции присваивания показано, что инициализировать локальную переменную с неявно заданным типом значением `null` все-таки можно, если в явном виде указать тип (в данном примере это тип `string`). 

---
### Ключевое слово *ref*

CLR также позволяет передавать параметры по ссылке, а не по значению. В C# это делается с помощью ключевых слов `out` и `ref`. Оба заставляют компилятор  генерировать метаданные, описывающие параметр как переданный по ссылке. 

С точки зрения CLR, ключевые слова `out` и `ref` не различаются, то есть для них генерируются одинаковый IL-код, а метаданные отличаются всего одним битом, указывающим, какое ключевое слово было использовано при объявлении метода. Однако компилятор C# различает эти ключевые слова при выборе метода, используемого для инициализации объекта, на который указывает переданная ссылка. Если параметр метода помечен ключевым словом `out`, вызывающий код может не инициализировать его, пока не вызван сам метод. В этом случае вызванный метод не может прочитать значение параметра и должен записать его, прежде чем вернуть управление. Если же параметр помечен ключевым словом ref, вызывающий код должен инициализировать его перед вызовом метода, а вызванный метод может как читать, так и записывать значение параметра. 

Поведение ссылочных и значимых типов при использовании ключевых слов out и ref различается значительно. Вот как это выглядит в случае значимого типа:
```C#
public sealed class Program
{
    public static void Main()
    {
        int num;                // Инициализация num 
        GetVal(out num);        // Инициализация num не обязательна 
        Console.WriteLine(num); // Выводится 10 
    }

    private static void GetVal(out int val)
    {
        val = 10;             // Этот метод должен инициализировать переменную val 
    }
}
```

 Здесь переменная `num` объявлена в стеке `Main`, а ее адрес передается методу `GetVal`. Параметр этого метода `val` представляет собой указатель на значимый тип `Int32`. Внутри метода `GetVal` значение типа `Int32`, на которое указывает `val`, изменяется на 10. Именно это значение выводится на консоль, когда метод `GetVal` возвращает управление. Использование ключевого слова `out` со значимыми типами повышает эффективность кода, так как предотвращает копирование экземплярных полей значимого типа при вызовах методов. 
 
 А теперь рассмотрим аналогичный пример с ключевым словом `ref`: 
```C#
public sealed class Program
{
    public static void Main()
    {
        int num = 5;              // Инициализация x
        AddVal(ref num);          // num требуется инициализировать
        Console.WriteLine(num);   // Выводится 15
    }
    private static void AddVal(ref int val)
    {
        val += 10;          
    }
}
```

 Здесь объявленной в стеке `Main` переменной `num` присваивается начальное значение 5. Затем ее адрес передается методу `AddVal`, параметр `val` которого представляет собой указатель на значимый тип `Int32` в стеке `Main`. Внутри метода `AddVal` должно быть уже инициализированное значение типа `Int32`, на которое указывает параметр `val`. Таким образом, метод `AddVal` может использовать первоначальное значение `val` в любом выражении. Он может менять это значение, возвращая вызывающему коду новый вариант. В рассматриваемом примере метод `AddVal` прибавляет к исходному значению 10. Соответственно, когда он возвращает управление, переменная `num` метода `Main` содержит значение 15, которое и выводится на консоль. 
 
 Важно отметить, что с точки зрения IL или CLR ключевые слова `out` и `ref` ничем не различаются: оба заставляют передать указатель на экземпляр объекта. Разница в том, что они помогают компилятору гарантировать корректность кода. 
 
 >Вопрос: Почему при вызовах методов в программах на C# надо в явном виде указывать ключевые слова out или ref, если компилятор в состоянии самостоятельно определить, какое из ключевых слов ему требуется, а значит, должен корректно компилировать код?
 >
   Разработчики C# сочли, что вызывающий код должен явно указывать свои намерения, чтобы при вызове метода сразу было ясно, что этот метод должен менять значение передаваемой переменной. 
   Кроме того, CLR позволяет по-разному перегружать методы в зависимости от выбора параметра out или ref. Например, следующий код на C# вполне допустим и прекрасно компилируется: 
```C#
public sealed class Point 
{ 
	static void Add(Point p) { ... } 
	static void Add(ref Point p) { ... } 
}
```
> Не допускается перегружать методы, отличающиеся только типом параметров (`out` или `ref`), так как результатом их JIT-компиляции становится идентичный код метаданных, представляющих сигнатуру методов. Поэтому в показанном ранее типе `Point` нельзя определить вот такой метод:
```C#
static void Add(out Point p) { ... }
```
>  При попытке включить такой метод в тип Point компилятор C# вернет ошибку): 
```C#
error CS0663: 'Add' cannot define overloaded methods that differ only on ref and out 
```

 Со значимыми типами ключевые слова `out` и `ref` дают тот же результат, что и передача ссылочного типа по значению. Они позволяют методу управлять единственным экземпляром значимого типа. Вызывающий код должен выделить память для этого экземпляра, а вызванный метод управляет выделенной памятью. В случае ссылочных типов вызывающий код выделяет память для указателя на передаваемый объект, а вызванный код управляет этим указателем. В силу этих особенностей использование ключевых слов `out` и `ref` со ссылочными типами полезно, лишь когда метод собирается «вернуть» ссылку на известный ему объект. Рассмотрим это на примере:
```C#
public sealed class Program
{
    public static void Main()
    {
        // Обязательное присвоение начального значения null
        FileStream fs = null; 
        ProcessFiles(ref fs);

        // Продолжаем, пока остаются необработанные файлы
        for (; fs != null; ProcessFiles(ref fs))
        {
            fs.Read(...);    // Обработка файла
        }
    }
    private static void ProcessFiles(ref FileStream fs)
    {
        // Если предыдущий файл открыт, закрываем его
        if (fs != null) fs.Close(); // Закрыть последний обрабатываемый файл

        // Открыть следующий файл или вернуть null, если файлов больше нет
        if (noMoreFilesToProcess) fs = null;
        else fs = new FileStream(...);
    }
}
```

---
### Ключевое слово *dynamic*

При написания приложения на  C# многие разработчики используют его для связи с компонентами, не реализованными на С#. Некоторые из этих компонентов 
могут быть написаны на динамических языках, например Python или Ruby, или быть [COM-объектами](https://learn.microsoft.com/ru-ru/windows/win32/com/the-component-object-model), которые поддерживают интерфейс IDispatch (возможно, реализованный на С или C++), или объектами модели DOM (Document Object Model), реализованными при помощи разных языков и технологий. Взаимодействие с DOM-объектами особенно полезно для построения Silverlight-приложений.

Для того чтобы облегчить разработку при помощи рефлексии или коммуникаций с другими компонентами, компилятор С# предлагает помечать типы как динамические (dynamic). Вы также можете записывать результаты вычисления выражений в переменную и пометить ее тип как динамический. Затем динамическое выражение (переменная) может быть использовано для вызовов членов класса, например поля, свойства/индексатора, метода, делегата, или унарных/бинарных операторов. Когда исполняемый код вызывает член класса при помощи динамического выражения (переменной), компилятор создает специальный IL-код, который описывает желаемую операцию. Этот код называется полезной нагрузкой (payload). Во время выполнения программы он определяет существующую операцию для выполнения на основе действительного типа объекта, на который ссылается динамическое выражение (переменная). 

Следующий код поясняет, о чем идет речь:
```C#
class Programm
{
    public static void Main(string[] args)
    {
        dynamic value;

        for (var demo = 0; demo < 2; demo++)
        {
            value = (demo == 0) ? (dynamic)5 : (dynamic)"A";
            value = value + value;
            Print(value);
        }
    }

    static void Print(int num) => Console.WriteLine("Output (Int32):  " + num);

    static void Print(string str) => Console.WriteLine("Output (String): " + str);
}
```

Концепция `dynamic` в C# предоставляет гибкость и удобство при работе с объектами, чьи типы известны только во время выполнения программы. Вот несколько реальных примеров использования `dynamic`:

1. **Работа с данными из динамических источников**:
   При работе с данными из динамических источников, таких как данные из базы данных, JSON или XML файлов, типы данных могут быть неизвестны заранее. Использование `dynamic` позволяет упростить обработку таких данных:

   ```csharp
   dynamic jsonData = GetJsonData();
   dynamic databaseData = GetDatabaseData();

   var result1 = jsonData["key"];
   var result2 = databaseData.SomeMethod();
   ```

2. **Использование средств динамических языков .NET**:
   Взаимодействие с средствами, написанными на динамических языках .NET, таких как `IronPython` или `IronRuby`, также является хорошим примером использования `dynamic`. В этом случае, объекты, созданные в этих языках, могут передаваться и использоваться в C# коде без необходимости заранее известных типов:

   ```csharp
   dynamic pythonObject = GetPythonObject();
   var result = pythonObject.SomeMethod();
   ```

3. **Рефлексия**:
   При работе с рефлексией `dynamic` может быть полезным для упрощения доступа к членам объекта, когда точный тип объекта неизвестен на этапе написания кода:

   ```csharp
   dynamic obj = GetObjectUsingReflection();
   var result = obj.SomeMethod();
   ```

4. **Интеграция с динамическими библиотеками и API**:
   Использование `dynamic` может облегчить интеграцию с динамическими библиотеками и API, где типы данных могут быть динамически определены во время выполнения:

   ```csharp
   dynamic apiResponse = CallDynamicAPI();
   var result = apiResponse.SomeProperty;
   ```

5. **Работа с динамически создаваемыми объектами**:
   При создании объектов динамически во время выполнения программы, использование `dynamic` может упростить их использование без явного определения типа:

   ```csharp
   dynamic newObject = CreateDynamicObject();
   var result = newObject.SomeProperty;
   ```

В этих примерах использования `dynamic` код становится более гибким и адаптируемым к изменяющимся условиям и требованиям вашей программы. Однако следует использовать `dynamic` осторожно, поскольку утрата статической типизации может привести к ухудшению производительности и потенциальным ошибкам во время выполнения программы.