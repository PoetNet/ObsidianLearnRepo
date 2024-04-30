#TODO
## Base C# 
1. Keyword unsafe.
2. Hashset. Сложность поиска элемента.
3. Obj --> ValueType.
4. GetHashCode(). Hash и HashCode (Число - элемент индекса в массиве).
5. Операторы in, ref, out.
6. Таблица интернирования строк.
7. Кооперативная многозадачность. (Корутины в Python).
8. Lock. Memmory Barrier.
9. InterLocked.
10. Semaphore / SemaphoreSlim & Lock difference. (Semaphore can be used async).
11. Машина состояний.
12. Async initialization context. Task.ConigureAwait(false).
13. SpinLock. SpinWait.
14. Context switching.
15.  Batching LINQ (MoreLINQ?).
16. Virtual. Позднее связывание.
17. Task & TaskValue.
18. CancelationToken. HttpContext.RequestAborted.
19. IAsyncDisposable().

## EF Core
1. Expressions. Expression tree (where used, LINQ).
2. Iquerryable (vs IEnumerable). #Important
3. Write LEFT JOIN.
4. Request optimization.
5. Track Entities.
6. As split querry.
7. ToList (Происходит создание и заполнение списка).
8. Indexes types.
9. 1+N problem.

## PostgresSQL
1. Connections limit.

## ASP.NET Core
1. Action filters.
2. Вызвать действия перед и после обработки запроса без middleware.
3. Варианты DI (конструктор, методы, свойства). Связанность и связность.
4. IMiddleware & Conventional Middleware. (Захват зависимости).
5. StartUp.cs (ConfigureServices & Configure) & Programm.cs.
6. InLineMiddleware (не через класс). HealthCheck.
7. IServiceScopeFactory.CreateScope() vs IServiceProvider.CreateScope() extension. Dispose.

## xUnit / nUnit
1. Range, Act, Assert.
2. Fluent.Asertion.
3. Insomnia ?

## Технологии
1. Reddis (кэш). TryGet().
		1.1. InMemoryCash.
1. Борьба с неконсистентностью. 
2. HangFire.
3. NPOI (Для работы с XLSX документами).
   
## Брокеры сообщений
1. RabbitMQ (Exchange, Queue). - 3 статьи на habr.
2. InBox / OutBox. (Сохранение сообщений в РБД).

## Архитектура
1. SOA (поставщик услуг, сервисный реестр,  пользователи услуг).
2. DDD (все валидации на уровне моделей, все конфигурации рядом)
   
## Что я такое?
 1. TypeObj. В нем ссылка на static methods.
 2. CQRS (Regular, Progressive, Deluxe).
 3. Валидация данных.
	 3.1. Fluent API.
		 - гибкость
		 - тесты, метод 
	 3.2. Attributes. (Runtime with Reflection). Чекнуть можно ли применить один атрибут к другому.
		 - сложно тестировать
		 - нет типобезопасности 
	 3.3. Data.Annotations.NectCore
4. Mediator.
5. Quatrz.

## CC
1. Доменная аутентификация.
## Терминология
1. Мультитенантность.
2. Аспектно-ориентированное программирование.


## ВАЖНО!
1. Mediator Не нужен! [Андрей Парамонов "MediatR не нужен" (youtube.com)](https://www.youtube.com/watch?v=isrl_zJa1GY)
2. HttpLogging - встроенное логирование от Microsoft.
3. OpenTelemetry.

## Аналитика
1. Характеристика вытеснения