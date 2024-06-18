
1. System.Threading.LockRecursionException: 'Recursive write lock acquisitions not allowed in this mode.'
- Комментарий тимлида: "Из-за того, что ты lock два раза взял"
Yказывает на проблему с повторным входом в блокировку. Ошибка возникает, когда `ReaderWriterLockSlim` пытается захватить блокировку записи, пока уже захвачена блокировка чтения, или наоборот, в той же нити выполнения. В вашем случае проблема связана с тем, что вы пытаетесь войти в блокировку чтения в `finally` блоке, что не требуется и приводит к ошибке.

```csharp
    private List<SomeComponent> _components = new List<SomeComponent>();
    public IReadOnlyList<SomeComponent> Components
    {
        get
        {
            _lock.EnterReadLock();
            try
            {
	            // Solution of the problem
                return _components.AsReadOnly(); 
            }
            finally
            {
                _lock.EnterReadLock();
            }
        }
        private set
        {
            _lock.EnterWriteLock();
            try
            {
                _components = value.ToList();
            }
            finally
            {
                _lock.ExitWriteLock();
            }
        }
    }
    
```

Решение: 
```csharp
private readonly ReaderWriterLockSlim _lock = new(LockRecursionPolicy .SupportsRecursion);
``` 
