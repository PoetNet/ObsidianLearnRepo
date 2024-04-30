#CleanCode #SOLID #DIP

Принцип инверсии зависимостей (Dependency Inversion Principle) служит для создания слабосвязанных сущностей, которые легко тестировать, модифицировать и обновлять. Этот принцип можно сформулировать следующим образом:

> Модули верхнего уровня не должны зависеть от модулей нижнего уровня. И те и другие должны зависеть от абстракций.
> Абстракции не должны зависеть от деталей. Детали должны зависеть от абстракций.

Рассмотрим следующий пример:

```C#
class Book
{
    public string Text { get; set; }
    public ConsolePrinter Printer { get; set; }

    public void Print() => Printer.Print(Text);
}

class ConsolePrinter
{
    public void Print(string text) => Console.WriteLine(text);
}
```

Класс `Book`, представляющий книгу, использует для печати класс `ConsolePrinter`. При подобном определении класс `Book` зависит от класса `ConsolePrinter`. Более того мы жестко определили, что печать книгу можно только на консоли с помощью класса `ConsolePrinter`. Другие же варианты, например, вывод на принтер, вывод в файл или с использованием каких-то элементов графического интерфейса - все это в данном случае исключено. Абстракция печати книги не отделена от деталей класса `ConsolePrinter`. Все это является нарушением принципа инверсии зависимостей.

Теперь приведем классы в соответствие с принципом инверсии зависимостей, отделив абстракции от низкоуровневой реализации:

```C#
interface IPrinter
{
    void Print(string text);
}

class Book(IPrinter printer)
{
    public string Text { get; set; }

    public void Print() => printer.Print(Text);
}

class ConsolePrinter : IPrinter
{
    public void Print(string text) => Console.WriteLine("Печать на консоли");
}

class HtmlPrinter : IPrinter
{
    public void Print(string text) => Console.WriteLine("Печать в html");
}
```

Теперь абстракция печати книги отделена от конкретных реализаций. В итоге и класс `Book` и класс `ConsolePrinter` зависят от абстракции `IPrinter`.