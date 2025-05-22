# История языка C# 
Нововведения в разных версиях языка C#. Текст сгенерирован с помощью `DeepSeek`.  
После чего был исправлен и дополнен ссылками и примерами.    
Подробнее можно прочитать у профильных блогеров: [блогер 1](https://andrey.moveax.ru/), [блогер 2](https://www.thomasclaudiushuber.com/blog/), [блогер 3](https://pvs-studio.ru/ru/blog/posts/csharp/), [блогер 4](https://metanit.com/sharp/tutorial/23.1.php), [блогер 5](https://endjin.com/what-we-think/editions/dotnet-development).  
Также есть похожая статья на сайте [Микрософт](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-version-history), на сайте [Википедии](https://ru.wikipedia.org/wiki/C_Sharp), ответ в [Stack Overflow](https://stackoverflow.com/questions/247621/what-are-the-correct-version-numbers-for-c/247623#247623), вехи в [github](https://github.com/dotnet/csharplang/milestones).  
Для практики можно воспользоваться [бесплатными курсами](https://learn.microsoft.com/ru-ru/training/browse/?products=dotnet) от Микрософт.

От себя добавлю, что в проекте можно менять версию .NET, но нельзя менять версию языка C#.  
Скажем, Visual Studio 2019 (2022) выставляет версию языка в 7.3 для классического .NET Framework любой версии.   
Visual Studio 2022 для .NET Core 3, 5, 6, 7, 8, 9 выставляет версию языка 8, 9, 10, 11, 12, 13 соотвественно.  
Уточнить версию компилятора можно вставив в исходники строку ```#error version```.   
В статье я рассматриваю только изменения синтаксиса языка, нововведения в стандартной библиотеке не упоминаются.  

---

### **C# 1.0 (январь 2002)**
- **Основные нововведения**:
  - Базовые функции языка: [классы](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/types/classes), [структуры](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/struct), [интерфейсы](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/types/interfaces), [делегаты](https://learn.microsoft.com/ru-ru/dotnet/csharp/delegates-overview), [события](https://learn.microsoft.com/ru-ru/dotnet/csharp/events-overview), [атрибуты](https://learn.microsoft.com/ru-ru/dotnet/csharp/advanced-topics/reflection-and-attributes), [исключения](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/exceptions/), [пространства имен](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/types/namespaces)
  - Поддержка ООП (наследование, полиморфизм, инкапсуляция): [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/tutorials/oop)
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
  - Обобщения (Generics): [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/generics/)
  - Анонимные методы: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/statements-expressions-operators/anonymous-methods)
  - Статические классы: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/static-classes-and-static-class-members)
  - Итераторы (`yield`): [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/iterators)
  - Nullable типы: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/nullable-value-types)
  - Ковариация и контравариация в делегатах: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/covariance-contravariance/)
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
  - LINQ (Language Integrated Query): [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/linq/)
  - Лямбда-выражения: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/lambda-expressions)
  - Ключевое слово `var`: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/statements/declarations#implicitly-typed-local-variables)
  - Анонимные типы: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/anonymous-types)
  - Методы расширения: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/extension-methods)
  - Автоматические свойства: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/auto-implemented-properties)
  - Инициализаторы объектов и коллекций: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/object-and-collection-initializers)
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
  - Динамическая типизация (`dynamic`): [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/reference-types#the-dynamic-type)
  - Именованные и необязательные аргументы: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/named-and-optional-arguments)
  - Ковариация и контравариация в обобщенных интерфейсах и делегатах: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/covariance-contravariance/)
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
  - Асинхронное программирование (`async`/`await`): [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/async/)
  - Информация о вызывающем методе (Caller Info Attributes): [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/attributes/caller-information)
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
  - Инициализаторы свойств: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/auto-implemented-properties#property-initializers)
  - Лямбда-выражения (`=>`) для определения метода: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/lambda-expressions)
  - Интерполяция строк (`$""`): [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/tokens/interpolated)
  - Оператор `nameof`: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/nameof)
  - Фильтры исключений (`when`): [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/when)
  - Null-условный оператор (`?.`): [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/member-access-operators#null-conditional-operators--and-)
  - Статические импорты (`using static`): [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/using-directive#the-static-modifier)
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
  - Сопоставление с образцом (`pattern matching`): [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/patterns)
  - Локальные функции: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/local-functions)
  - `ref` возвращаемые значения и локальные переменные: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/ref#reference-return-values)
  - Упрощенное объявление `out` переменных: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/out-parameter-modifier#calling-a-method-with-an-out-argument)
  - Бинарные литералы и целочисленные разделители: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/integral-numeric-types#integer-literals)
  - Больше возвращаемых типов в асинхронных методах: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/asynchronous-programming/async-return-types)
  - Лямбда-выражения для остальных частей класса: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/statements-expressions-operators/expression-bodied-members)
  - `throw` выражения: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/statements/exception-handling-statements#the-throw-expression)
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
  - Асинхронный `Main`: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/program-structure/main-command-line)
  - Вывод типов для `default`: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/default#default-literal)
  - Вывод имени члена кортежа: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/value-tuples#tuple-field-names)
  - Именованные параметры конструктора атрибута: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/advanced-topics/reflection-and-attributes/#attribute-parameters)
  - Сопоставление с образцом для обобщений: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/functional/pattern-matching#type-tests)
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
  - Модификатор `in`: [документация](https://docs.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/in-parameter-modifier)
  - Упрощение в именованных аргументах: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/named-and-optional-arguments)
  - Безопасный `stackalloc`: [документация](https://docs.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/stackalloc)
  - `private protected`: [документация](https://docs.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/private-protected)
  - Условные `ref`-выражения: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/conditional-operator#conditional-ref-expression)
  - Модификатор возвращаемого значения `ref readonly`: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/readonly#ref-readonly-return-example)
  - Неизменяемые структуры `readonly struct`: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/struct#readonly-struct)
  - Стековые структуры `ref struct`: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/ref-struct)
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
  - Сравнение кортежей: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/value-tuples#tuple-equality)
  - Новые ограничения параметра типа: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/generics/constraints-on-type-parameters#unmanaged-constraint)
  - Переназначение локальных ссылок `ref`: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/statements/declarations#reference-variables)
  - Улучшения `fixed`: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/statements/fixed)
  - Улучшения `stackalloc`: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/stackalloc)
- **Версия .NET**: .NET Framework 4.8, .NET Core 2.1.
- **Версия Visual Studio**: Visual Studio 2017 (15.7+).

подробнее с примерами: [часть 1](https://andrey.moveax.ru/post/csharp-features-v7-3-managed), [часть 2](https://andrey.moveax.ru/post/csharp-features-v7-3-unmanaged)

<details><summary>Пример кода C# 7.3</summary>

```csharp
using System;

public class Example
{
    public static void Main()
    {
        int x = 5, y = 10;
        ref int r = ref x;
        r = ref y;
        r = 42;
        var tuple = (x, y);
        Console.WriteLine(tuple == (5, 42));
    }
}
```
</details>

---

### **C# 8.0 (сентябрь 2019)**
- **Основные нововведения**:
  - Nullable ссылочные типы: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/nullable-references)
  - Асинхронные потоки (`IAsyncEnumerable`): [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/asynchronous-programming/generate-consume-asynchronous-stream)
  - Индексы и диапазоны: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/tutorials/ranges-indexes)
  - Реализация по умолчанию в интерфейсах: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/interfaces/explicit-interface-implementation)
  - Выражение `switch`: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/switch-expression)
  - Улучшенное сопоставление шаблонов: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/patterns)
  - Декларации `using`: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/proposals/csharp-8.0/using)
  - Статические локальные функции: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/static)
  - `readonly` члены в структурах: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/struct#readonly-instance-members)
  - Присваивание при значении `null`: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/null-coalescing-operator)
- **Версия .NET**: .NET Core 3.0, .NET Standard 2.1.
- **Версия Visual Studio**: Visual Studio 2019.

подробнее с примерами: [часть 1](https://andrey.moveax.ru/post/csharp-features-v8-0-small-features), [часть 2](https://andrey.moveax.ru/post/csharp-features-v8-0-nullable-reference-types), [часть 3](https://andrey.moveax.ru/post/csharp-features-v8-0-async-streams), [часть 4](https://andrey.moveax.ru/post/csharp-features-v8-0-default-implementation-for-interface-methods), [часть 5](https://andrey.moveax.ru/post/csharp-features-v8-0-indexes-and-ranges), [часть 6](https://andrey.moveax.ru/post/csharp-features-v8-0-new-switch-expression), [часть 7](https://andrey.moveax.ru/post/csharp-features-v8-0-using-declarations).

<details><summary>Пример кода C# 8.0</summary>

```csharp
#nullable enable

using System;
using System.Collections.Generic;
using System.Threading.Tasks;

internal class Program
{
    static async Task Main(string[] args)
    {
        int[]? arr = null;
        var list = new List<int>();
        await foreach (var i in GetIntAsync())
            list.Add(i);
        arr ??= list.ToArray()[2..7];

        ProcessRange(null, arr);
        ProcessRange("null", null);
        ProcessRange("empty", Array.Empty<int>());

        static async IAsyncEnumerable<int> GetIntAsync()
        {
            for (int i = 0; i < 10; i++)
                yield return i;
        }

        static void ProcessRange(string? title, int[]? range)
        {
            var mess = range switch
            {
                null => "null",
                _ when range.Length == 0 => "empty",
                _ => string.Join(", ", range)
            };
            Console.WriteLine(@$"{title ?? "range"}: {mess}");
        }
    }
}
```
</details>

---

### **C# 9.0 (ноябрь 2020)**
- **Основные нововведения**:
  - Операторы верхнего уровня: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/program-structure/top-level-statements)
  - Записи (Records): [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/record)
  - Инициализаторы только для чтения (`init`): [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/object-and-collection-initializers)
  - Выражения `with` для записей: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/with-expression)
  - Сопоставление с образцом для логических выражений: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/proposals/csharp-9.0/patterns3)
- **Версия .NET**: .NET 5.0.
- **Версия Visual Studio**: Visual Studio 2019 (16.8+).

подробнее с примерами: [часть 1](https://andrey.moveax.ru/post/csharp-features-v9-0-top-level-statement), [часть 2](https://andrey.moveax.ru/post/csharp-features-v9-0-pattern-matching), [часть 3](https://andrey.moveax.ru/post/csharp-features-v9-0-records-basics), [часть 4](https://andrey.moveax.ru/post/csharp-features-v9-0-records-advanced), [часть 5](https://andrey.moveax.ru/post/csharp-features-v9-0-init-only-setter).

<details><summary>Пример кода C# 9.0</summary>

```csharp
using System;
using System.Linq;
using System.Net.Http;
using System.Text.Json;

var client = new HttpClient();
var data = await client.GetStringAsync("https://jsonplaceholder.typicode.com/posts");
var posts = JsonSerializer.Deserialize<Post[]>(data)!;
var firstPost = posts.Where(p => p is { id:  1 }).First();
Console.WriteLine(firstPost);
var copyPost = firstPost with { title = "New title" };
copyPost.body = "New body";
Console.WriteLine(copyPost);

record Post (int id, int userId, string title)
{
    public string body { get; set; }

    public override string ToString() =>
        $"id: {id}, userId: {userId}, title: {Trunc(title, 20)}, body: {Trunc(body?.Replace('\n', ' '), 20)}";

    string Trunc(string str, int len) =>
        str?.Length > len ? str[..len] + "..." : str;
}
```
</details>

---

### **C# 10.0 (ноябрь 2021)**
- **Основные нововведения**:
  - Улучшенные структуры: инициализация свойств и полей, конструктор без параметров, `with`
  - Структуры записей (`record struct`): [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/record)
  - Глобальные `using`-директивы: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/using-directive#the-global-modifier)
  - Упрощенное объявление пространств имен: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/namespace#using-statements-in-file-scoped-namespaces)
  - Расширенные шаблоны свойств: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/proposals/csharp-10.0/extended-property-patterns)
  - Естественный тип ламбда-выражения: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/lambda-expressions#explicit-return-type)
  - Явный возвращаемый тип лямбда-выражения: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/lambda-expressions#explicit-return-type)
  - Константные интерполируемые строки
- **Версия .NET**: .NET 6.0.
- **Версия Visual Studio**: Visual Studio 2022.

[подробнее с примерами](https://pvs-studio.ru/ru/blog/posts/csharp/0875/)

<details><summary>Пример кода C# 10.0</summary>

```csharp
global using System;
namespace CSharp10Sample;

readonly record struct Number(int value)
{
    public override string ToString() => value.ToString();
}

class Program
{
    static void Main()
    {
        var n = new Number();
        n = n with { value = 10 };
        var square = Number (Number n) => new Number(n.value * n.value);
        const string title = $"Just for information";
        Console.WriteLine($"{title}: {n} * {n} = {square(n)}");
    }
}
```
</details>

---

### **C# 11.0 (ноябрь 2022)**
- **Основные нововведения**:
  - Необработанные строковые литералы (`"""`): [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/strings/#raw-string-literals)
  - Модификатор `file` для типов: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/file)
  - Обязательный модификатор `required`: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/required)
  - Универсальные атрибуты: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-11#generic-attributes)
  - Шаблоны списков: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/patterns#list-patterns)
  - Строковые литералы `UTF-8`: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/reference-types#utf-8-string-literals)
  - Статические абстрактные (`static abstract`) и статические виртуальные (`static virtual`) члены интерфейса: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/interface#static-abstract-and-virtual-members)
- **Версия .NET**: .NET 7.0.
- **Версия Visual Studio**: Visual Studio 2022 (17.4+).

[подробнее с примерами](https://metanit.com/sharp/tutorial/23.1.php), [на сайте микрософт](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-11)

<details><summary>Пример кода C# 11.0</summary>

```csharp
var container = ContainerString.Create("""Apple, Cherry""");
var items = container.List switch
{
    ["Apple, Plum"] => "Apple and Plum",
    [.., "Cherry"] => "Last Cherry",
    _ => "Unknown container"
};
Console.WriteLine(items);

file interface IContainer<T> where T: IContainer<T>
{
    static abstract T Create(string data);
}

file class ContainerString : IContainer<ContainerString>
{
    public required List<string> List { get; init; }
    public static ContainerString Create(string data) => new()
    {
        List = data
            .Split(",")
            .Select(s => s.Trim())
            .ToList()
    };
}
```
</details>

---

### **C# 12.0 (ноябрь 2023)**
- **Основные нововведения**:
  - Первичные конструкторы: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/instance-constructors#primary-constructors)
  - Выражения коллекции: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/collection-expressions)
  - Параметры лямбда-кода по умолчанию: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/lambda-expressions#input-parameters-of-a-lambda-expression)
  - Модификатор `ref readonly` для параметров метода: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/method-parameters#ref-readonly-modifier)
  - Псевдоним любого типа: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/using-directive#the-using-alias)
- **Версия .NET**: .NET 8.0.
- **Версия Visual Studio**: Visual Studio 2022 (17.8+).

[подробнее с примерами](https://metanit.com/sharp/tutorial/23.2.php), [на сайте микрософт](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-12)

<details><summary>Пример кода C# 12.0</summary>

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Runtime.CompilerServices;
using IntContainerInfo = (int Count, int Sum);

IntContainer data = [1, 2, 3];
Span<int> add = stackalloc int[] { 4, 1 };
add[0] = 5;
data = data.Concat(ref add).Exclude(1);
Console.WriteLine($"Content: {data}, Info: {data.Info}");

[CollectionBuilder(typeof(IntContainer), "Create")]
public class IntContainer(ReadOnlySpan<int> items): IEnumerable<int>
{
    private readonly int[] _items = items.ToArray();
    public static IntContainer Create(ReadOnlySpan<int> items) => new(items);
    public IntContainer Concat(ref readonly Span<int> items) => new([.. _items, .. items]);
    public IntContainer Exclude(int v = 0) => new(_items.Where(i => i != v).ToArray());
    public IntContainerInfo Info => (_items.Count(), _items.Sum());
    public override string ToString() => $"[{string.Join(", ", _items)}]";
    public IEnumerator<int> GetEnumerator() => _items.AsEnumerable().GetEnumerator();
    IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
}

```
</details>

---

### **C# 13.0 (ноябрь 2024)**
- **Основные нововведения**:
  - Коллекции `params`: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/method-parameters#params-modifier)
  - `\e` escape-последовательность: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-13#new-escape-sequence)
  - Оператор `^` в инициализаторе объекта с индексом: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-13#implicit-index-access)
  - Частичные свойства и индексаторы: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/partial-member)
  - `ref` и `unsafe` в итераторах и `async` методах: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-13#ref-and-unsafe-in-iterators-and-async-methods)
  - Реализация интерфейсов в `ref struct`: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-13#ref-struct-interfaces)
  - Приоритет разрешения перегрузки: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/attributes/general#overloadresolutionpriority-attribute)
  - Анти-ограничение параметра типа `allows ref struct`: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/generics/constraints-on-type-parameters#allows-ref-struct) 
- **Версия .NET**: .NET 9.0.
- **Версия Visual Studio**: Visual Studio 2022 (17.12+).

[подробнее с примерами](https://metanit.com/sharp/tutorial/23.3.php), [на сайте микрософт](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-13)

<details><summary>Пример кода C# 13.0</summary>

```csharp
using System;
using System.Runtime.CompilerServices;

class Program
{
    partial class ExtendableArray<T>
    {
        int _count;
        T[] _data;

        public ExtendableArray(int capacity = 0) => _data = new T[_count = capacity];

        [OverloadResolutionPriority(1)]
        public partial T this[Index i] { get; set; }
        public T this[int i] => _data[i];

        public override string ToString() => $"[{string.Join(", ", new ArraySegment<T>(_data, 0, _count))}]";
    }

    partial class ExtendableArray<T>
    {

        public partial T this[Index i]
        {
            get
            {
                CheckSize(i.GetOffset(_data.Length) + 1);
                return _data[i];
            }
            set
            {
                CheckSize(i.GetOffset(_data.Length) + 1);
                _data[i] = value;
            }
        }

        private void CheckSize(int size)
        {
            _count = Math.Max(_count, size);
            
            bool skip = _data.Length >= size;
            string start = skip ? "\e[32m" : "\e[31m";
            string end = "\e[0m";
            Console.WriteLine($"CheckSize {size}, skip={start}{skip}{end}");
            if (skip)
                return;
            
            int capacity = _data.Length == 0 ? 1 : _data.Length;
            while (capacity < size)
                capacity *= 2;

            var data = _data;
            _data = new T[capacity];
            data.CopyTo(_data, 0);
        }
    }

    public static void Main()
    {
        var list = new ExtendableArray<int>(3)
        {
            [0] = 1,
            [^1] = 2
        };
        list[3] = 3;
        list[4] = 1 + list[3];
        Console.WriteLine(list);
    }
}
```
</details>

---

### **C# 14.0 Preview (релиз ожидается в конце 2025)**
- **Основные нововведения**:
  - Члены расширения: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-14#extension-members)
  - Условное присваивание `null`: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-14#extension-members)
  - `nameof` поддерживает несвязанные универсальные типы: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-14#extension-members)
  - Более неявные преобразования для `Span<T>` и `ReadOnlySpan<T>`: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-14#implicit-span-conversions)
  - Модификаторы простых лямбда-параметров: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-14#simple-lambda-parameters-with-modifiers)
  - `field` поддерживаемые свойства: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-14#the-field-keyword)
  - `partial` события и конструкторы: [документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-14#more-partial-members)
- **Версия .NET**: .NET 10.0.
- **Версия Visual Studio**: Visual Studio 2022.

[на сайте микрософт](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-14)

[//]: [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Fmiptleha%2Fcs-versions&count_bg=%230C7DBD&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
