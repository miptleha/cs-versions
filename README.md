# История языка C# 
Нововведения в разных версиях языка C#.  
Текст сгенерирован с помощью `DeepSeek`.  
Подробнее можно прочитать у профильных блогеров: [блогер 1](https://andrey.moveax.ru/), [блогер 2](https://www.thomasclaudiushuber.com/blog/), [блогер 3](https://pvs-studio.ru/ru/blog/posts/csharp/), [блогер 4](https://metanit.com/sharp/tutorial/23.1.php), [блогер 5](https://endjin.com/what-we-think/editions/dotnet-development).  
Также есть похожая статья на сайте [Микрософт](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-version-history) и на сайте [Википедии](https://ru.wikipedia.org/wiki/C_Sharp).

---

### **C# 1.0 (январь 2002)**
- **Основные нововведения**:
  - Базовые функции языка: [классы](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/types/classes), [структуры](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/struct), [интерфейсы](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/types/interfaces), [делегаты](https://learn.microsoft.com/ru-ru/dotnet/csharp/delegates-overview), [события](https://learn.microsoft.com/ru-ru/dotnet/csharp/events-overview), [атрибуты](https://learn.microsoft.com/ru-ru/dotnet/csharp/advanced-topics/reflection-and-attributes), [исключения](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/exceptions/)
  - Поддержка ООП (наследование, полиморфизм, инкапсуляция): [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/tutorials/oop)
  - [Управляемый код](https://learn.microsoft.com/ru-ru/dotnet/standard/managed-code) и [сборка мусора](https://learn.microsoft.com/ru-ru/dotnet/standard/garbage-collection)
- **Поддерживаемые версии .NET**: .NET Framework 1.0.
- **Поддерживаемые версии Visual Studio**: Visual Studio .NET (2002).

<details>
  <summary>Пример кода C# 1.0</summary>
  ```charp
  using System;

namespace CSharp_1
{
    class Program
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
                    ShapeCreated(string.Format("Создана фигура: {0}, {1}, периметр: {2:0.##}, площадь: {3:0.##}", shape.Name, shape.Info(), shape.CalcPerimeter(), shape.CalcArea()));
            }
        }

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

### **C# 2.0 (ноябрь 2005)**
- **Основные нововведения**:
  - **Обобщения (Generics)**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/generics/)
  - **Анонимные методы**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/statements-expressions-operators/anonymous-methods)
  - **Итераторы (`yield`)**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/iterators)
  - **Nullable типы**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/nullable-value-types)
  - **Ковариация и контравариация в делегатах**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/covariance-contravariance/)
- **Поддерживаемые версии .NET**: .NET Framework 2.0.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2005.

---

### **C# 3.0 (ноябрь 2007)**
- **Основные нововведения**:
  - **LINQ (Language Integrated Query)**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/linq/)
  - **Лямбда-выражения**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/lambda-expressions)
  - **Анонимные типы**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/anonymous-types)
  - **Методы расширения**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/extension-methods)
  - **Автоматические свойства**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/auto-implemented-properties)
  - **Инициализаторы объектов и коллекций**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/object-and-collection-initializers)
- **Поддерживаемые версии .NET**: .NET Framework 3.5.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2008.

---

### **C# 4.0 (апрель 2010)**
- **Основные нововведения**:
  - **Динамическая типизация (`dynamic`)**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/reference-types#the-dynamic-type)
  - **Именованные и необязательные аргументы**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/named-and-optional-arguments)
  - **Ковариация и контравариация в обобщенных интерфейсах и делегатах**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/covariance-contravariance/)
- **Поддерживаемые версии .NET**: .NET Framework 4.0.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2010.

---

### **C# 5.0 (август 2012)**
- **Основные нововведения**:
  - **Асинхронное программирование (`async`/`await`)**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/async/)
  - **Информация о вызывающем методе (Caller Info Attributes)**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/attributes/caller-information)
- **Поддерживаемые версии .NET**: .NET Framework 4.5.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2012.

---

### **C# 6.0 (июль 2015)**
- **Основные нововведения**:
  - **Инициализаторы свойств**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/auto-implemented-properties#property-initializers)
  - **Лямбда-выражения (`=>`)**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/lambda-expressions)
  - **Интерполяция строк (`$""`)**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/tokens/interpolated)
  - **Оператор `nameof`**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/nameof)
  - **Фильтры исключений (`when`)**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/when)
  - **Null-условный оператор (`?.`)**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/member-access-operators#null-conditional-operators--and-)
- **Поддерживаемые версии .NET**: .NET Framework 4.6, .NET Core 1.0.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2015.

---

### **C# 7.0 (март 2017)**
- **Основные нововведения**:
  - **Кортежи и деконструкция**: [кортежи](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/value-tuples), [деконструкция](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/functional/deconstruct)
  - **Сопоставление с образцом (`pattern matching`)**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/patterns)
  - **Локальные функции**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/local-functions)
  - **`ref` возвращаемые значения и локальные переменные**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/ref#reference-return-values)
  - **Упрощенное объявление `out` переменных**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/out-parameter-modifier#calling-a-method-with-an-out-argument)
- **Поддерживаемые версии .NET**: .NET Framework 4.7, .NET Core 2.0.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2017.

---

### **C# 8.0 (сентябрь 2019)**
- **Основные нововведения**:
  - **Nullable ссылочные типы**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/nullable-references)
  - **Асинхронные потоки (`IAsyncEnumerable`)**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/asynchronous-programming/generate-consume-asynchronous-stream)
  - **Индексы и диапазоны**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/tutorials/ranges-indexes)
  - **Реализация по умолчанию в интерфейсах**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/interfaces/explicit-interface-implementation)
- **Поддерживаемые версии .NET**: .NET Core 3.0, .NET Standard 2.1.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2019.

---

### **C# 9.0 (ноябрь 2020)**
- **Основные нововведения**:
  - **Записи (Records)**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/record)
  - **Инициализаторы только для чтения (`init`)**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/object-and-collection-initializers)
  - **Выражения `with` для записей**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/with-expression)
  - **Сопоставление с образцом для логических выражений**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/proposals/csharp-9.0/patterns3)
- **Поддерживаемые версии .NET**: .NET 5.0.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2019 (16.8+).

---

### **C# 10.0 (ноябрь 2021)**
- **Основные нововведения**:
  - **Глобальные using-директивы**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/using-directive#the-global-modifier)
  - **Упрощенное объявление пространств имен**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/namespace#using-statements-in-file-scoped-namespaces)
  - **Расширенные шаблоны свойств**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/proposals/csharp-10.0/extended-property-patterns)
- **Поддерживаемые версии .NET**: .NET 6.0.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2022.

---

### **C# 11.0 (ноябрь 2022)**
- **Основные нововведения**:
  - **Новые строки в интерполяциях строк**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-11#newlines-in-string-interpolations)
  - **Универсальные атрибуты**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-11#generic-attributes)
  - **Улучшения сопоставления с образцом**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/functional/pattern-matching)
- **Поддерживаемые версии .NET**: .NET 7.0.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2022 (17.4+).

---

### **C# 12.0 (ноябрь 2023)**
- **Основные нововведения**:
  - **Первичные конструкторы**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-12#primary-constructors)
  - **Выражения коллекции**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-12#collection-expressions)
  - **Параметры лямбда-кода по умолчанию**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-12#default-lambda-parameters)
- **Поддерживаемые версии .NET**: .NET 8.0.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2022 (17.8+).

---

### **C# 13.0 (ноябрь 2024)**
- **Основные нововведения**:
  - **Коллекции params**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-13#params-collections)
  - **Неявный доступ к индексу**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-13#implicit-index-access)
  - **Поддержка модификатора unsafe**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-13#ref-and-unsafe-in-iterators-and-async-methods)
  - **Частичные свойства и индексаторы в partial типах**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-13#more-partial-members)
  - **Приоритет разрешения перегрузки**: [обзор](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-13#overload-resolution-priority)
- **Поддерживаемые версии .NET**: .NET 9.0.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2022 (17.12+).

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Fmiptleha%2Fcs-versions&count_bg=%230C7DBD&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
