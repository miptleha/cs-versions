# История языка C# 
Нововведения в разных версиях языка C#. Текст сгенерирован с помощью `DeepSeek`.  
После чего был исправлен и дополнен ссылками и примерами.    
Подробнее можно прочитать у профильных блогеров: [блогер 1](https://andrey.moveax.ru/), [блогер 2](https://www.thomasclaudiushuber.com/blog/), [блогер 3](https://pvs-studio.ru/ru/blog/posts/csharp/), [блогер 4](https://metanit.com/sharp/tutorial/23.1.php), [блогер 5](https://endjin.com/what-we-think/editions/dotnet-development).  
Также есть похожая статья на сайте [Микрософт](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-version-history), на сайте [Википедии](https://ru.wikipedia.org/wiki/C_Sharp), ответ в [Stack Overflow](https://stackoverflow.com/questions/247621/what-are-the-correct-version-numbers-for-c/247623#247623), вехи в [github](https://github.com/dotnet/csharplang/milestones).

От себя добавлю, что в проекте можно менять версию .NET, но нельзя менять версию языка C#.  
Скажем, Visual Studio 2019 выставляет версию языка в 7.3 для классического .NET Framework любой версии.   
Таким образом, версия C# привязана к версии Visual Studio, а не к версии .NET.  
Уточнить версию компилятора можно вставив в исходники строку ```#error version```.   
В статье я рассматриваю только изменения синтаксиса языка, нововведения в стандартной библиотеке не упоминаются.  

---

### **C# 1.0 (январь 2002)**
- **Основные нововведения**:
  - Базовые функции языка: [классы](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/types/classes), [структуры](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/struct), [интерфейсы](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/types/interfaces), [делегаты](https://learn.microsoft.com/ru-ru/dotnet/csharp/delegates-overview), [события](https://learn.microsoft.com/ru-ru/dotnet/csharp/events-overview), [атрибуты](https://learn.microsoft.com/ru-ru/dotnet/csharp/advanced-topics/reflection-and-attributes), [исключения](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/exceptions/), [пространства имен](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/types/namespaces)
  - Поддержка ООП (наследование, полиморфизм, инкапсуляция): [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/tutorials/oop)
  - [Управляемый код](https://learn.microsoft.com/ru-ru/dotnet/standard/managed-code) и [сборка мусора](https://learn.microsoft.com/ru-ru/dotnet/standard/garbage-collection)
- **Версия .NET**: .NET Framework 1.0.
- **Версия Visual Studio**: Visual Studio .NET 2002.

<details><summary>Пример кода C# 1.0</summary>

```csharp
using System;

namespace CSharp_1
{
    interface IShape
    {
        string Name { get; }
        double CalcArea();
        double CalcPerimeter();
        string Info();
    }

    class Rectangle : IShape
    {
        double _width, _height;
        public Rectangle(double width, double height)
        {
            if (width <= 0 || height <= 0)
                throw new ArgumentException("Сторона должна быть больше 0");
            _width = width;
            _height = height;
        }
        public string Name { get { return "Прямоугольник"; } }

        public double CalcArea()
        {
            return _width * _height;
        }

        public double CalcPerimeter()
        {
            return 2 * (_width + _height);
        }

        public string Info()
        {
            return string.Format("ширина: {0:0.##}, высота: {1:0.##}", _width, _height);
        }
    }

    class Circle : IShape
    {
        double _radius;
        public Circle(double radius)
        {
            _radius = radius;
        }
        public string Name { get { return "Круг"; } }

        public double CalcArea()
        {
            return Math.PI * _radius * _radius;
        }

        public double CalcPerimeter()
        {
            return 2 * Math.PI * _radius;
        }

        public string Info()
        {
            return string.Format("радиус: {0:0.##}", _radius);
        }
    }

    delegate void ShapeEventHandler(string message);
    class ShapeManager
    {
        public event ShapeEventHandler ShapeCreated;

        public void CreateShape(IShape shape)
        {
            if (ShapeCreated != null)
            {
                ShapeCreated(string.Format(
                    "Создана фигура: {0}, {1}, периметр: {2:0.##}, площадь: {3:0.##}", 
                    shape.Name, shape.Info(), shape.CalcPerimeter(), shape.CalcArea()
                ));
            }
        }
    }

    class Program
    {
        static void ShowInfo(string message)
        {
            Console.WriteLine(message);
        }

        public static void Main()
        {
            try
            {
                ShapeManager sm = new ShapeManager();
                sm.ShapeCreated += new ShapeEventHandler(ShowInfo);

                Rectangle rect = new Rectangle(10, 20);
                sm.CreateShape(rect);

                Circle circle = new Circle(10.1010);
                sm.CreateShape(circle);

                Rectangle invalidRect = new Rectangle(-20, -30);
                sm.CreateShape(invalidRect);
            }
            catch (ArgumentException ex)
            {
                Console.WriteLine("Ошибка: " + ex.Message);
            }
            catch (Exception ex)
            {
                Console.WriteLine("Неизвестная ошибка: " + ex.ToString());
            }
        }
    }
}
```
</details>

---

### **C# 1.2 (апрель 2003)**
- **Основные нововведения**:
  - `foreach` вызывает `Dispose`
  - Исправления и оптимизация
- **Версия .NET**: .NET Framework 1.1.
- **Версия Visual Studio**: Visual Studio .NET 2003.

<details><summary>Пример кода C# 1.2</summary>

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Text;

class Range : IEnumerable, IEnumerator, IDisposable
{
    int[] _data;
    int _pos = -1;

    public Range(int start, int end)
    {
        if (end < start)
            throw new Exception("Invalid range");
        _data = new int[end - start];
        for (int i = 0; i < end - start; i++)
            _data[i] = i + start;
    }
    public object Current { get { return _data[_pos]; } }

    public void Dispose()
    {
        Console.WriteLine("Dispose"); //not called in c# 1.0
    }

    public IEnumerator GetEnumerator()
    {
        this.Reset();
        return this;
    }

    public bool MoveNext()
    {
        _pos++;
        return _pos < _data.Length;
    }

    public void Reset()
    {
        _pos = -1;
    }
}

class Program
{
    static void Main(string[] args)
    {
        IEnumerable r = new Range(1, 3);
        foreach (var i in r)
            Console.WriteLine(i);
        foreach (var i in r)
            Console.WriteLine(i);
    }
}
```
</details>

---

### **C# 2.0 (ноябрь 2005)**
- **Основные нововведения**:
  - Обобщения (Generics): [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/generics/)
  - Анонимные методы: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/statements-expressions-operators/anonymous-methods)
  - Статические классы: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/static-classes-and-static-class-members)
  - Итераторы (`yield`): [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/iterators)
  - Nullable типы: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/nullable-value-types)
  - Ковариация и контравариация в делегатах: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/covariance-contravariance/)
- **Версия .NET**: .NET Framework 2.0.
- **Версия Visual Studio**: Visual Studio 2005.

<details><summary>Пример кода C# 2.0</summary>

```csharp
using System;
using System.Collections.Generic;

namespace CSharp_2
{
    delegate void DoAction<T>(T item);

    static class CollectionUtils
    {
        public static List<T> CreateList<T>(params T[] items)
        {
            return new List<T>(items);
        }

        public static IEnumerable<T> Reverse<T>(IEnumerable<T> items)
        {
            List<T> buffer = new List<T>(items);
            for (int i = buffer.Count - 1; i >= 0; i--)
                yield return buffer[i];
        }

        public static void ForEach<T>(IEnumerable<T> items, DoAction<T> action, bool reverse)
        {
            if (reverse)
                items = Reverse(items);
            foreach (T item in items)
                action(item);
        }
    }

    class Program
    {
        public static void Main()
        {
            List<int?> list = CollectionUtils.CreateList<int?>(1, 2, null, 4, 5);
            CollectionUtils.ForEach(list, delegate (int? i)
            {
                Console.WriteLine(i * i ?? default(int));
            }, true);
        }
    }
}
```
</details>

---

### **C# 3.0 (ноябрь 2007)**
- **Основные нововведения**:
  - LINQ (Language Integrated Query): [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/linq/)
  - Лямбда-выражения: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/lambda-expressions)
  - Ключевое слово `var`: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/statements/declarations#implicitly-typed-local-variables)
  - Анонимные типы: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/anonymous-types)
  - Методы расширения: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/extension-methods)
  - Автоматические свойства: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/auto-implemented-properties)
  - Инициализаторы объектов и коллекций: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/object-and-collection-initializers)
- **Версия .NET**: .NET Framework 3.5.
- **Версия Visual Studio**: Visual Studio 2008.

<details><summary>Пример кода C# 3.0</summary>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

namespace CSharp_3
{
    class Person
    {
        public string Name { get; set; }
        public int Age { get; set; }
    }

    static class PersonExtension
    {
        public static bool IsAdult(this Person p)
        {
            return p.Age >= 18;
        }
    }

    class Program
    {
        public static void Main()
        {
            var list = new List<Person>
            {
                new Person { Name = "Alex", Age = 10},
                new Person { Name = "Ivan", Age = 20},
                new Person { Name = "Peter", Age = 30}
            };

            var filtered = list.Where(p => p.IsAdult()).Select(p => new { p.Name });
            foreach (var p in filtered)
                Console.WriteLine(p.Name); 
        }
    }
}
```
</details>

---

### **C# 4.0 (апрель 2010)**
- **Основные нововведения**:
  - Динамическая типизация (`dynamic`): [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/reference-types#the-dynamic-type)
  - Именованные и необязательные аргументы: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/named-and-optional-arguments)
  - Ковариация и контравариация в обобщенных интерфейсах и делегатах: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/covariance-contravariance/)
- **Версия .NET**: .NET Framework 4.0.
- **Версия Visual Studio**: Visual Studio 2010.

<details><summary>Пример кода C# 4.0</summary>

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

namespace ConsoleApp13
{
    class Program
    {
        class Bot
        {
            public void Hello(string prefix = "") { Console.WriteLine(prefix + "Hello"); }
            public void Bye(string prefix = "") { Console.WriteLine(prefix + "Bye"); }
        }

        static void Main(string[] args)
        {
            var bots = new Bot[5];
            for (int i = 0; i < bots.Length; i++)
                bots[i] = new Bot();
            Parallel.For(0, bots.Length, i => Print(bots[i], num: i, delay: 100));
        }

        static void Print(dynamic obj, int num, int delay = 10)
        {
            obj.Hello("Bot " + num + " say: ");
            Thread.Sleep(delay);
            obj.Bye("Bot " + num + " say: ");
        }

    }
}
```
</details>

---

### **C# 5.0 (август 2012)**
- **Основные нововведения**:
  - Асинхронное программирование (`async`/`await`): [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/async/)
  - Информация о вызывающем методе (Caller Info Attributes): [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/attributes/caller-information)
- **Версия .NET**: .NET Framework 4.5.
- **Версия Visual Studio**: Visual Studio 2012.

<details><summary>Пример кода C# 5.0</summary>

```csharp
using System;
using System.Runtime.CompilerServices;
using System.Threading.Tasks;

public enum LogLevel { Debug, Info, Warning, Error }

public static class Logger
{
    public static void Log(
        string message,
        LogLevel level = LogLevel.Info,
        [CallerMemberName] string callerName = "",
        [CallerFilePath] string callerFilePath = "",
        [CallerLineNumber] int callerLineNumber = 0)
    {
        string timestamp = DateTime.Now.ToString("yyyy-MM-dd HH:mm:ss.fff");
        string fileName = System.IO.Path.GetFileName(callerFilePath);
        string levelStr = level.ToString().ToUpper();

        Console.WriteLine($"[{timestamp}] [{levelStr}] [{callerName}() in {fileName}:{callerLineNumber}] {message}");
    }
}

public static class ConsoleApplication
{
    public static void Main()
    {
        Logger.Log("Начало программы", LogLevel.Debug);
        MethodAsync().GetAwaiter().GetResult();
    }

    private static async Task MethodAsync()
    {
        Logger.Log(">MethodAsync");
        await Task.Delay(100);
        Logger.Log("<MethodAsync");
    }
}
```
</details>

---

### **C# 6.0 (июль 2015)**
- **Основные нововведения**:
  - Инициализаторы свойств: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/auto-implemented-properties#property-initializers)
  - Лямбда-выражения (`=>`) для определения метода: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/lambda-expressions)
  - Интерполяция строк (`$""`): [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/tokens/interpolated)
  - Оператор `nameof`: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/nameof)
  - Фильтры исключений (`when`): [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/when)
  - Null-условный оператор (`?.`): [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/member-access-operators#null-conditional-operators--and-)
- **Версия .NET**: .NET Framework 4.6, .NET Core 1.0.
- **Версия Visual Studio**: Visual Studio 2015.

[подробнее с примерами](https://andrey.moveax.ru/post/csharp-new-festures-in-csharp-6)

<details><summary>Пример кода C# 6.0</summary>

```csharp
using System;
using static System.Console;

public class Person
{
    public string Name { get; } = "Anonymous";
    public int Age { get; set; } = 18;

    public Person(string name) => Name = name;

    public override string ToString() => $"Name: {Name}, Age: {Age}";
}

public class Program
{
    public static void Main()
    {
        try
        {
            var person = new Person("Alice");
            person.Age = -1;
            WriteLine(person);        // "Name: Alice, Age: -1"

            // Проверка nameof (используется для безопасного получения имени переменной)
            ValidatePerson(person);
        }
        catch (Exception ex) when (ex.Message.Contains("Age")) // Фильтр исключений
        {
            WriteLine($"Проблема с возрастом: {ex.Message}");
        }
        catch (Exception ex)
        {
            WriteLine($"Ошибка: {ex.Message}");
        }
    }

    // Метод с использованием nameof для валидации
    public static void ValidatePerson(Person person)
    {
        if (person == null)
            throw new ArgumentNullException(nameof(person)); // Безопасное имя параметра

        if (person.Age < 0)
            throw new ArgumentException("Age cannot be negative", nameof(person.Age));
    }
}
```
</details>

---

### **C# 7.0 (март 2017)**
- **Основные нововведения**:
  - Кортежи и деконструкция: [кортежи](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/value-tuples), [деконструкция](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/functional/deconstruct)
  - Сопоставление с образцом (`pattern matching`): [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/patterns)
  - Локальные функции: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/local-functions)
  - `ref` возвращаемые значения и локальные переменные: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/ref#reference-return-values)
  - Упрощенное объявление `out` переменных: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/out-parameter-modifier#calling-a-method-with-an-out-argument)
  - Бинарные литералы и целочисленные разделители: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/integral-numeric-types#integer-literals)
  - Больше возвращаемых типов в асинхронных методах: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/asynchronous-programming/async-return-types)
  - Лямбда-выражения для остальных частей класса: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/statements-expressions-operators/expression-bodied-members)
  - `throw` выражения: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/statements/exception-handling-statements#the-throw-expression)
- **Версия .NET**: .NET Framework 4.7, .NET Core 2.0.
- **Версия Visual Studio**: Visual Studio 2017.

[подробнее с примерами](https://itvdn.com/ru/blog/article/csharp7)

<details><summary>Пример кода C# 7.0</summary>

```csharp
using System;

class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y) => (X, Y) = (x, y);
    public void Deconstruct(out int x, out int y) => (x, y) = (X, Y);
}

class Program
{
    static void PrintPoints(Point[] points)
    {
        foreach (var p in points)
            PrintPoint(p);

        void PrintPoint(Point p) => Console.WriteLine(p.X + " " + p.Y);
    }

    static ref Point GetPoint(Point[] points)
    {
        return ref points[0];
    }

    static void Main()
    {
        int.TryParse("1", out int val);
        Point[] points = { new Point(val, 2_0), new Point(3, 4) };
        ref var p = ref GetPoint(points);
        p = new Point(5, 6);
        var (x, y) = p;
        PrintPoints(points);

        switch (points[0])
        {
            case Point point when point.X == x:
                Console.WriteLine("Test ok");
                break;
        }
    }
}
```
</details>

---

### **C# 7.1 (август 2017)**
- **Основные нововведения**:
  - Асинхронный `Main`: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/program-structure/main-command-line)
  - Вывод типов для `default`: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/default#default-literal)
  - Вывод имени члена кортежа: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/value-tuples#tuple-field-names)
  - Именованные параметры конструктора атрибута: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/advanced-topics/reflection-and-attributes/#attribute-parameters)
  - Сопоставление с образцом для обобщений: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/functional/pattern-matching#type-tests)
- **Версия .NET**: .NET Framework 4.7.1, .NET Core 2.0.
- **Версия Visual Studio**: Visual Studio 2017 (15.3+).

[подробнее с примерами](https://andrey.moveax.ru/post/csharp-features-v7-1)

<details><summary>Пример кода C# 7.1</summary>

```csharp
using System;
using System.Threading.Tasks;

class Program
{
    [Obsolete(message: "Этот метод устарел")]
    static async Task Main()
    {
        int count = 1000;
        string msg = default;
        var t = (count, msg);
        await Task.Delay(t.count);
    }
}
```
</details>

---

### **C# 7.2 (декабрь 2017)**
- **Основные нововведения**:
  - Модификатор `in`: [обзор](https://docs.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/in-parameter-modifier)
  - Упрощение в именованных аргументах: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/named-and-optional-arguments)
  - Безопасный `stackalloc`: [обзор](https://docs.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/stackalloc)
  - `private protected`: [обзор](https://docs.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/private-protected)
  - Условные `ref`-выражения: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/conditional-operator#conditional-ref-expression)
  - Модификатор возвращаемого значения `ref readonly`: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/readonly#ref-readonly-return-example)
  - Неизменяемые структуры `readonly struct`: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/struct#readonly-struct)
  - Стековые структуры `ref struct`: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/ref-struct)
- **Версия .NET**: .NET Framework 4.7.2, .NET Core 2.0.
- **Версия Visual Studio**: Visual Studio 2017 (15.5+).

[подробнее с примерами](https://andrey.moveax.ru/post/csharp-features-v7-2)

<details><summary>Пример кода C# 7.2</summary>

```csharp
using System;

readonly struct Point
{
    public Point(int x, int y) => (X, Y) = (x, y);
    public int X { get; }
    public int Y { get; }
    public override string ToString() => $"({X}, {Y})";
    public double DistanceTo(Point p) => Math.Sqrt(Math.Pow(X - p.X, 2) + Math.Pow(Y - p.Y, 2));
}

class Program
{
    static void Main(string[] args)
    {
        ref readonly Point GetClosest(in Point t, in Point a, in Point b) =>
            ref (t.DistanceTo(a) < t.DistanceTo(b) ? ref a : ref b);

        Point target = new Point(1, -1);
        Point[] points = new Point[] { new Point(2, 3), new Point(3, 2) };
        ref readonly var closest = ref GetClosest(t: target, points[0], b: points[1]);

        Console.WriteLine($"Точка {target}");
        Console.WriteLine($"Массив точек: {String.Join(", ", points)}");
        Console.WriteLine($"Ближайшая точка в массиве: {closest}");
    }
}
```
</details>

---

### **C# 7.3 (май 2018)**
- **Основные нововведения**:
  - Сравнение кортежей: [обзор](https://docs.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-7-3#tuple-equality)
  - Улучшения `fixed`: [обзор](https://docs.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-7-3#indexing-fixed-fields)
  - Оптимизация `stackalloc`: [обзор](https://docs.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-7-3#stackalloc-arrays)
- **Версия .NET**: .NET Framework 4.8, .NET Core 2.1.
- **Версия Visual Studio**: Visual Studio 2017 (15.7+).

[подробнее с примерами](https://andrey.moveax.ru/post/csharp-features-v7-3-managed)

---

### **C# 8.0 (сентябрь 2019)**
- **Основные нововведения**:
  - Nullable ссылочные типы: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/nullable-references)
  - Асинхронные потоки (`IAsyncEnumerable`): [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/asynchronous-programming/generate-consume-asynchronous-stream)
  - Индексы и диапазоны: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/tutorials/ranges-indexes)
  - **Реализация по умолчанию в интерфейсах**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/interfaces/explicit-interface-implementation)
- **Версия .NET**: .NET Core 3.0, .NET Standard 2.1.
- **Версия Visual Studio**: Visual Studio 2019.

[подробнее с примерами](https://andrey.moveax.ru/post/csharp-features-v8-0-small-features)

---

### **C# 9.0 (ноябрь 2020)**
- **Основные нововведения**:
  - Записи (Records): [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/record)
  - Инициализаторы только для чтения (`init`): [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/object-and-collection-initializers)
  - Выражения `with` для записей: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/with-expression)
  - Сопоставление с образцом для логических выражений: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/proposals/csharp-9.0/patterns3)
- **Версия .NET**: .NET 5.0.
- **Версия Visual Studio**: Visual Studio 2019 (16.8+).

[подробнее с примерами](https://andrey.moveax.ru/post/csharp-features-v8-0-small-features)

---

### **C# 10.0 (ноябрь 2021)**
- **Основные нововведения**:
  - Глобальные `using`-директивы: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/using-directive#the-global-modifier)
  - Упрощенное объявление пространств имен: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/namespace#using-statements-in-file-scoped-namespaces)
  - Расширенные шаблоны свойств: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/proposals/csharp-10.0/extended-property-patterns)
- **Версия .NET**: .NET 6.0.
- **Версия Visual Studio**: Visual Studio 2022.

[подробнее с примерами](https://pvs-studio.ru/ru/blog/posts/csharp/0875/)

---

### **C# 11.0 (ноябрь 2022)**
- **Основные нововведения**:
  - Новые строки в интерполяциях строк: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-11#newlines-in-string-interpolations)
  - Универсальные атрибуты: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-11#generic-attributes)
  - Улучшения сопоставления с образцом: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/functional/pattern-matching)
- **Версия .NET**: .NET 7.0.
- **Версия Visual Studio**: Visual Studio 2022 (17.4+).

[подробнее с примерами](https://pvs-studio.ru/ru/blog/posts/csharp/1002/)

---

### **C# 12.0 (ноябрь 2023)**
- **Основные нововведения**:
  - Первичные конструкторы: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-12#primary-constructors)
  - Выражения коллекции: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-12#collection-expressions)
  - Параметры лямбда-кода по умолчанию: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-12#default-lambda-parameters)
- **Версия .NET**: .NET 8.0.
- **Версия Visual Studio**: Visual Studio 2022 (17.8+).

[подробнее с примерами](https://pvs-studio.ru/ru/blog/posts/csharp/1074/)

---

### **C# 13.0 (ноябрь 2024)**
- **Основные нововведения**:
  - Коллекции `params`: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-13#params-collections)
  - Неявный доступ к индексу: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-13#implicit-index-access)
  - Поддержка модификатора `unsafe`: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-13#ref-and-unsafe-in-iterators-and-async-methods)
  - Частичные свойства и индексаторы в partial типах: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-13#more-partial-members)
  - Приоритет разрешения перегрузки: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-13#overload-resolution-priority)
- **Версия .NET**: .NET 9.0.
- **Версия Visual Studio**: Visual Studio 2022 (17.12+).

[подробнее с примерами](https://metanit.com/sharp/tutorial/23.3.php)

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Fmiptleha%2Fcs-versions&count_bg=%230C7DBD&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
