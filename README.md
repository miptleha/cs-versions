# cs-versions
Нововведения в разных версиях языка C#
Текст сгенерирован с помощью `DeepSeek`.  
От себя добавил ссылки на блогеров, где они рассматривают нововведения.

---

### **C# 1.0 (январь 2002)**
- **Основные нововведения**:
  - Базовые функции языка: классы, структуры, интерфейсы, делегаты, события.
  - Поддержка ООП (наследование, полиморфизм, инкапсуляция).
  - Управляемый код и сборка мусора.
- **Ссылки на нововведения**: [Обзор C# 1.0](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-version-history#c-version-10)
- **Поддерживаемые версии .NET**: .NET Framework 1.0.
- **Поддерживаемые версии Visual Studio**: Visual Studio .NET (2002).

---

### **C# 2.0 (ноябрь 2005)**
- **Основные нововведения**:
  - **Обобщения (Generics)**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/generics/)
  - **Анонимные методы**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/statements-expressions-operators/anonymous-methods)
  - **Итераторы (`yield`)**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/iterators)
  - **Nullable типы**: [NДокументация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/nullable-value-types)
  - **Ковариация и контравариация в делегатах**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/covariance-contravariance/)
- **Поддерживаемые версии .NET**: .NET Framework 2.0.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2005.

---

### **C# 3.0 (ноябрь 2007)**
- **Основные нововведения**:
  - **LINQ (Language Integrated Query)**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/linq/)
  - **Лямбда-выражения**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/lambda-expressions)
  - **Анонимные типы**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/anonymous-types)
  - **Методы расширения**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/extension-methods)
  - **Автоматические свойства**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/auto-implemented-properties)
  - **Инициализаторы объектов и коллекций**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/object-and-collection-initializers)
- **Поддерживаемые версии .NET**: .NET Framework 3.5.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2008.

---

### **C# 4.0 (апрель 2010)**
- **Основные нововведения**:
  - **Динамическая типизация (`dynamic`)**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/reference-types#the-dynamic-type)
  - **Именованные и необязательные аргументы**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/named-and-optional-arguments)
  - **Ковариация и контравариация в обобщенных интерфейсах и делегатах**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/covariance-contravariance/)
- **Поддерживаемые версии .NET**: .NET Framework 4.0.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2010.

---

### **C# 5.0 (август 2012)**
- **Основные нововведения**:
  - **Асинхронное программирование (`async`/`await`)**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/async/)
  - **Информация о вызывающем методе (Caller Info Attributes)**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/attributes/caller-information)
- **Поддерживаемые версии .NET**: .NET Framework 4.5.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2012.

---

### **C# 6.0 (июль 2015)**
- **Основные нововведения**:
  - **Инициализаторы свойств**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/auto-implemented-properties#property-initializers)
  - **Лямбда-выражения (`=>`)**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/lambda-expressions)
  - **Интерполяция строк (`$""`)**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/tokens/interpolated)
  - **Оператор `nameof`**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/nameof)
  - **Фильтры исключений (`when`)**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/when)
  - **Null-условный оператор (`?.`)**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/member-access-operators#null-conditional-operators--and-)
- **Поддерживаемые версии .NET**: .NET Framework 4.6, .NET Core 1.0.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2015.

---

### **C# 7.0 (март 2017)**
- **Основные нововведения**:
  - **Кортежи и деконструкция**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/builtin-types/value-tuples), [Деконструкция](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/functional/deconstruct)
  - **Сопоставление с образцом (`pattern matching`)**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/patterns)
  - **Локальные функции**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/classes-and-structs/local-functions)
  - **`ref` возвращаемые значения и локальные переменные**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/ref#reference-return-values)
  - **Упрощенное объявление `out` переменных**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/language-reference/keywords/out-parameter-modifier#calling-a-method-with-an-out-argument)
- **Поддерживаемые версии .NET**: .NET Framework 4.7, .NET Core 2.0.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2017.

---

### **C# 8.0 (сентябрь 2019)**
- **Основные нововведения**:
  - **Nullable ссылочные типы**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/nullable-references)
  - **Асинхронные потоки (`IAsyncEnumerable`)**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-8#asynchronous-streams)
  - **Индексы и диапазоны**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-8#indices-and-ranges)
  - **Реализация по умолчанию в интерфейсах**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-8#default-interface-methods)
- **Поддерживаемые версии .NET**: .NET Core 3.0, .NET Standard 2.1.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2019.

---

### **C# 9.0 (ноябрь 2020)**
- **Основные нововведения**:
  - **Записи (Records)**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-9#record-types)
  - **Инициализаторы только для чтения (`init`)**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-9#init-only-setters)
  - **Выражения `with` для записей**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-9#with-expressions)
  - **Сопоставление с образцом для логических выражений**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-9#pattern-matching-enhancements)
- **Поддерживаемые версии .NET**: .NET 5.0.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2019 (16.8+).

---

### **C# 10.0 (ноябрь 2021)**
- **Основные нововведения**:
  - **Глобальные using-директивы**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-10#global-using-directives)
  - **Упрощенное объявление пространств имен**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-10#file-scoped-namespaces)
  - **Улучшения для записей (Records)**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-10#record-structs)
- **Поддерживаемые версии .NET**: .NET 6.0.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2022.

---

### **C# 11.0 (ноябрь 2022)**
- **Основные нововведения**:
  - **Сырые строковые литералы**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-11#raw-string-literals)
  - **Обобщенные атрибуты**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-11#generic-attributes)
  - **Улучшения сопоставления с образцом**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-11#pattern-matching-enhancements)
- **Поддерживаемые версии .NET**: .NET 7.0.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2022 (17.4+).

---

### **C# 12.0 (ноябрь 2023)**
- **Основные нововведения**:
  - **Первичные конструкторы**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-12#primary-constructors)
  - **Улучшения для коллекций**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-12#collection-expressions)
  - **Асинхронные потоки с отменой**: [Документация](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-12#async-streams-with-cancellation)
- **Поддерживаемые версии .NET**: .NET 8.0.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2022 (17.8+).

---

### **C# 13.0 (ноябрь 2024)**
- **Основные нововведения**:
  - **Коллекции params**: [Описание](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-13#params-collections)
  - **Неявный доступ к индексу**: [Описание](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-13#implicit-index-access)
  - **Поддержка модификатора unsafe**: [Описание](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-13#ref-and-unsafe-in-iterators-and-async-methods)
  - **Частичные свойства и индексаторы в partial типах**: [Описание](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-13#primary-constructors)
  - **Приоритет разрешения перегрузки**: [Описание](https://learn.microsoft.com/ru-ru/dotnet/csharp/whats-new/csharp-13#overload-resolution-priority)
- **Поддерживаемые версии .NET**: .NET 9.0.
- **Поддерживаемые версии Visual Studio**: Visual Studio 2022 (17.12+).

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Fmiptleha%2Fcs-versions&count_bg=%230C7DBD&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
