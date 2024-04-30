#### **Задача 1:** 
#Easy
// Дан массив чисел. Найти все четные, положительные числа.

```csharp
int[] numbers = { 1, -2, 3, -4, 5, -6, 7, -8, 9 };

var average = numbers.Where(x => x % 2 == 0 && x >= 0); //1
```

#### **Задача 2:**
#Advanced 
// Дан массив строк. Найдите все уникальные буквы во всех строках и выведите их в алфавитном порядке.

**Решение:**
```csharp
using System.Linq;

string[] words = { "hello", "world", "linq", "programming" };
 
var uniqueLetters = words.SelectMany(word => word.ToLower())
                .Distinct()
                .OrderBy(letter => letter);
```

#### **Задача 3:**
#Easy  
// Дан список студентов с их оценками по различным предметам. Выведите список студентов в порядке убывания среднего балла.

**Решение:**
```csharp
List<Student> students = new List<Student>
{
    new Student { Name = "Alice", Grades = new List<int> { 90, 85, 88 } },
    new Student { Name = "Bob", Grades = new List<int> { 76, 82, 70 } },
    new Student { Name = "Charlie", Grades = new List<int> { 95, 92, 98 } }
};

class Student
{
    public string Name { get; set; }
    public List<int> Grades { get; set; }
}

var sortedStudents = students.OrderByDescending(student => student.Grades.Average());
```

#### Задача 4:
#Easy 
// Найти все строки, которые содержат только уникальные символы (без повторений).

```C#
List<string> words = new List<string> { "hello", "world", "apple", "banana", "goodbye" };

var uniqueStrings = words.Where(word => word.Distinct().Count() == word.Length);
```


#### **Задача 5:**
#Easy 
// Дан словарь, содержащий информацию о продуктах и их ценах. Найдите самый дорогой продукт.

**Решение:**
```csharp
Dictionary<string, double> products = new Dictionary<string, double>
{
    { "Apple", 2.5 },
    { "Banana", 1.8 },
    { "Orange", 3.2 },
    { "Grapes", 4.0 }
};


var mostExpensiveProduct = products.OrderByDescending(pair => pair.Value).First(); 
```

#### **Задача 6, 7, 8, 9:**
#Easy 
// 1. Выбрать всех студентов из списка, у которых возраст больше 20 лет.
// 2. Выбрать всех студентов из списка, чьи имена начинаются на букву "A".
// 3. Выбрать все уникальные возраста студентов из списка.
// 4. Найти студента с самым высоким средним баллом

```C#
List<Student> students = new List<Student> 
{ 
	new Student { Name = "Alice", Age = 22, Grades = new List<int> { 90, 85, 88 }},
	new Student { Name = "Bob", Age = 18, Grades = new List<int> { 75, 80, 82 } }, 
	new Student { Name = "Charlie", Age = 25, Grades = new List<int> { 95, 92, 98 } },
	new Student { Name = "David", Age = 19, Grades = new List<int> { 70, 65, 68 } }, 
}; 

var result1 = students.Where(student => student.Age > 20);             // 1
var result2 = students.Where(student => student.Name.StartsWith("A")); // 2
var uniqueAges = students.Select(student => student.Age).Distinct();   // 3

var topGradeStudent = students.OrderByDescending(student => student.Grades.Average()).FirstOrDefault(); // 4


```

#### **Задача 10, 11:**
#Hard
// 1. Найти среднюю длину слов в строке, исключая знаки препинания.
// 2. Найти все слова в строке, которые содержат только гласные буквы.
// 3. Найти наиболее часто встречающееся слово в строке, исключая стоп-слова (например, "is", "a", "the" и т. д.).

```C#
var inputString = "LINQ (Language Integrated Query) is a powerful tool for querying and manipulating data in C#. It allows developers to write queries in a SQL-like syntax directly within C# code.";

// 1
var averageLength = inputString
    .Split(new char[] { ' ', '.', ',', '!', '?' }, StringSplitOptions.RemoveEmptyEntries)
    .Select(word => word.Length)
    .Average();

// 2
var vowels = new HashSet<char> { 'a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U' };

var wordsWithOnlyVowels = inputString
    .Split(new char[] { ' ', '.', ',', '!', '?' }, StringSplitOptions.RemoveEmptyEntries)
    .Where(word => word.All(letter => vowels.Contains(letter)));

// 3
string[] stopWords = { "is", "a", "the", "and", "for", "in", "to", "it", "on", "an", "of" };

var mostCommonWord = inputString
    .Split(new char[] { ' ', '.', ',', '!', '?' }, StringSplitOptions.RemoveEmptyEntries)
    .Where(word => !stopWords.Contains(word.ToLower()))
    .GroupBy(word => word.ToLower())
    .OrderByDescending(group => group.Count())
    .Select(group => group.Key)
    .FirstOrDefault();
```
