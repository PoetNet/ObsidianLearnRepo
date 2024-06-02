1. Классический способ добавления новых элементов в коллекции, являющиеся свойством класса:

```csharp 
	public void AddSources(Function[] sources)
    {
        if (Sources == null)
        {
            Sources = new List<Function>();
        }
        if (sources != null)
        {
            foreach (var s in sources)
            {
                Sources.Add(s);
            }
        }
    }```

2. Получение значения или default:
```csharp
    public List<Function> this[int depth] => _components.GetValueOrDefault(depth, []);

```

3. Перебор сразу двух коллекций в foreach
```csharp
var col1Andcol2 = collection1.Zip(col2);
foreach (var (c1, c2) in col1Andcol2)
{
//...
}
```
