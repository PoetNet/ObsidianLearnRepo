#CleanCode #SOLID #ISP

Принцип разделения интерфейсов (Interface Segregation Principle) относится к тем случаям, когда классы имеют слишком раздутый интерфейс, не все методы и свойства которого используются и могут быть востребованы. Таким образом, интерфейс получатся слишком избыточен.

Принцип разделения интерфейсов можно сформулировать так:

> Клиенты не должны вынужденно зависеть от методов, которыми не пользуются.

При нарушении этого принципа клиент, использующий некоторый интерфейс со всеми его методами, зависит от методов, которыми не пользуется, и поэтому оказывается восприимчив к изменениям в этих методах. В итоге мы приходим к жесткой зависимости между различными частями интерфейса, которые могут быть не связаны при его реализации.

Техники для выявления нарушения этого принципа:

- Слишком большие интерфейсы
- Компоненты в интерфейсах слабо согласованы (перекликается с принципом единой ответственности)
- Методы без реализации (перекликается с принципом Лисков)

В этом случае интерфейс класса разделяется на отдельные части, которые составляют раздельные интерфейсы. Затем эти интерфейсы независимо друг от друга могут применяться и изменяться. В итоге применение принципа разделения интерфейсов делает систему слабосвязанной, и тем самым ее легче модифицировать и обновлять.

Допустим у нас есть интерфейс отправки сообщения:

```C#
interface IMessage
{
    void Send();
    string Text { get; set; }
    string Subject { get; set; }
    string ToAddress { get; set; }
    string FromAddress { get; set; }
}
```

Интерфейс определяет все основное, что нужно для отправки сообщения: само сообщение, его тему, адрес отправителя и получателя и сам метод отправки. 
Пусть есть класс `EmailMessage`, который реализует этот интерфейс:

```C#
class EmailMessage : IMessage
{
    public string Subject { get; set; } = "";
    public string Text { get; set; } = "";
    public string FromAddress { get; set; } = "";
    public string ToAddress { get; set; } = "";

    public void Send() => Console.WriteLine($"Отправляем Email-сообщение: {Text}");
}
```

Класс `EmailMessage` выглядит целостно, вполне удовлетворяя принципу единственной ответственности. То есть с точки зрения связности (cohesion) здесь проблем нет.

Теперь определим класс, который бы отправлял данные по смс:

```C#
class SmsMessage : IMessage
{
    public string Text { get; set; } = "";
    public string FromAddress { get; set; } = "";
    public string ToAddress { get; set; } = "";
    public string Subject
    {
        get => throw new NotImplementedException();
        set => throw new NotImplementedException();
    }

    public void Send() => Console.WriteLine($"Отправляем Sms-сообщение: {Text}");
}
```

Здесь есть проблема: свойство `Subject`, которое определяет тему сообщения, при отправке смс не указывается, поэтому оно в данном классе не нужно. Таким образом, в классе `SmsMessage` появляется избыточная функциональность, от которой класс `SmsMessage` начинает зависеть.

Это не очень хорошо, но посмотрим дальше. Допустим, нам надо создать класс для отправки голосовых сообщений.

Класс голосовой почты также имеет отправителя и получателя, только само сообщение передается в виде звука, что на уровне C# можно выразить в виде массива байтов. И в этом случае было бы неплохо, если бы интерфейс `IMessage` включал бы в себя дополнительные свойства и методы для этого, например:

```C#
interface IMessage
{
    void Send();
    string Text { get; set; }
    string ToAddress { get; set; }
    string Subject { get; set; }
    string FromAddress { get; set; }

    byte[] Voice { get; set; }
}
```

Тогда класс голосовой почты `VoiceMessage` мог бы выглядеть следующим образом:

```C#
class VoiceMessage : IMessage
{
    public string ToAddress { get; set; } = string.Empty;
    public string FromAddress { get; set; } = string.Empty;
    public byte[] Voice { get; set; } = new byte[] { };
    public string Text
    {
        get => throw new NotImplementedException();
        set => throw new NotImplementedException();
    }
    public string Subject
    {
        get => throw new NotImplementedException();
        set => throw new NotImplementedException();
    }

    public void Send() => Console.WriteLine("Передача голосовой почты");
}

```

И здесь опять же мы сталкиваемся с ненужными свойствами. Плюс нам надо добавить новое свойство в предыдущие классы SmsMessage и EmailMessage, причем этим классам свойство Voice в принципе то не нужно. В итоге здесь мы сталкиваемся с явным нарушением принципа разделения интерфейсов.

Для решения возникшей проблемы нам надо выделить из классов группы связанных методов и свойств и определить для каждой группы свой интерфейс:

```C#
interface IMessage
{
    void Send();
    string ToAddress { get; set; }
    string FromAddress { get; set; }
}
interface IVoiceMessage : IMessage
{
    byte[] Voice { get; set; }
}
interface ITextMessage : IMessage
{
    string Text { get; set; }
}

interface IEmailMessage : ITextMessage
{
    string Subject { get; set; }
}

class VoiceMessage : IVoiceMessage
{
    public string ToAddress { get; set; } = string.Empty;
    public string FromAddress { get; set; } = string.Empty;

    public byte[] Voice { get; set; } = Array.Empty<byte>();
    public void Send() => Console.WriteLine("Передача голосовой почты");
}
class EmailMessage : IEmailMessage
{
    public string Text { get; set; } = string.Empty;
    public string Subject { get; set; } = string.Empty;
    public string FromAddress { get; set; } = string.Empty;
    public string ToAddress { get; set; } = string.Empty;

    public void Send() => Console.WriteLine("Отправляем по Email сообщение: {Text}");
}

class SmsMessage : ITextMessage
{
    public string Text { get; set; } = string.Empty;
    public string FromAddress { get; set; } = string.Empty;
    public string ToAddress { get; set; } = string.Empty;
    public void Send() => Console.WriteLine("Отправляем по Sms сообщение: {Text}");
}
```
