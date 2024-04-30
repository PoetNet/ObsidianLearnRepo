 
```C#
MyClass obj = new MyClass();

MethodInfo method = typeof(MyClass).GetMethod("MyMethod");
object[] parameters = new object[] { 42, "Hello, World!", true };
method.Invoke(obj, parameters);

ConstructorInfo constructor = typeof(MyClass).GetConstructor(new[] { typeof(int), typeof(string) });
object[] parameters = new object[] { 42, "Hello, World!" };
MyClass obj = (MyClass)constructor.Invoke(parameters);
```
