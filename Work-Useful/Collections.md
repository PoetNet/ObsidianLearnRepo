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
