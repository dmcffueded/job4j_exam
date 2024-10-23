### 1. Что такое lambda-выражение?

В программировании термин лямбда обозначает возможность передать в качестве параметра блок кода. Переданный блок кода может быть вызван в любой момент времени или не вызван вообще. Лямбда в Java сводится к упрощению записи анонимного класса. Лямбда удобно использовать, когда функция описывается в одну строчку. Иногда вычисление может занять несколько строчек. В этом случае лямбда описывается через блок.

### 2. Что такое функциональные интерфейсы?

Функциональный интерфейс - это интерфейс, который содержит только один абстрактный метод. Абстрактный метод - это метод в котором нет реализации.

Для того, чтобы подчеркнуть, что интерфейс является функциональным, используется аннотация @FunctionalInterface. При этом, как и аннотация @Override она не является обязательной, код будет работать и без нее. Однако, она позволяет произвести дополнительную валидацию нашего кода - если мы попытаемся добавить в такой интерфейс еще один абстрактный метод, то на стадии компиляции мы получим ошибку.

Стоит подчеркнуть следующую особенность функциональных интерфейсов - они появились в Java версии 8 и после этого структура интерфейсов претерпела некоторые изменения:

- в интерфейсе можно добавлять статические методы и методы с реализацией по умолчанию (помечаются ключевым словом default). Так вот в функциональном интерфейсе таких методов может быть сколько угодно. Такие методы уже будут содержать реализацию;

- в функциональном интерфейсе может быть также дополнительно объявлено один или несколько абстрактных методов, которые по сигнатуре совпадают с методом из класса java.lang.Object, в таком случае они не влияют на ограничение в один абстрактный метод для функционального интерфейса. Такое вы можете увидеть в интерфейсе Comparator, если обратитесь к исходникам.

В разделе java.lang.Comparable и java.util.Comparator - это функциональные интерфейсы. Частым способом применения функциональных интерфейсов является их применение в шаблонах проектирования при рефакторинге нашего кода.

### 3. Перечислите функциональные интерфейсы из пакета java.util.function.

Java имеет пакет java.util.function, который содержит все необходимые функциональные интерфейсы. Если вы создаете свой функциональный интерфейс, то что-то в вашей программе не так.

Есть следующие встроенные функциональные интерфейсы:

1. Supplier. Supplier (поставщик) используется для создания какого-либо объекта и при этом ему не требуется входной параметр.

2. Consumer (BiConsumer). Consumer (потребитель) используется в том случае, если нам нужно применить какое-то действие или операцию к параметру (для BiConsumer два параметра) и при этом у метода нет возвращаемого значения.

3. Predicate (BiPredicate). Predicate (утверждение) наиболее часто применяется в фильтрах и сравнении.

4. Function (BiFunction). Function используется для преобразования входного параметра или двух параметров (для Bi-формы этого функционального интерфейса) в какое-либо значение, тип возвращаемого значения может не совпадать с типом входных параметров.

5. UnaryOperator и BinaryOperator. UnaryOperator и BinaryOperator – это разновидность Function, в которых входные и выходные обобщенные параметры должны совпадать.

### 4. Что такое функции высшего порядка?

Функции высшего порядка - это функции, зависящие от других функций.

В программировании функции высшего порядка описываются через композицию.

Например. Композиция - объект А не может существовать без объекта B.

Автомобиль и колеса - это композиция.

### 5. Какие функциональные интерфейсы из пакета java.util.function поддерживают функции высшего порядка?

В пакете `java.util.function` функциональные интерфейсы, которые поддерживают функции высшего порядка, включают те, которые могут принимать или возвращать другие функции. Вот несколько из них:

**`Function<T, R>`**
- Метод `andThen(Function<? super R, ? extends V> after)` позволяет объединять функции, выполняя сначала одну, затем другую:
  ```java
  Function<Integer, Integer> multiplyBy2 = x -> x * 2;
  Function<Integer, Integer> add3 = x -> x + 3;
  Function<Integer, Integer> combined = multiplyBy2.andThen(add3);
  System.out.println(combined.apply(5)); // Результат: 13 (5 * 2 + 3)
  ```
- Метод `compose(Function<? super V, ? extends T> before)` позволяет объединять функции, выполняя сначала другую, затем эту:
  ```java
  Function<Integer, Integer> multiplyBy2 = x -> x * 2;
  Function<Integer, Integer> add3 = x -> x + 3;
  Function<Integer, Integer> combined = multiplyBy2.compose(add3);
  System.out.println(combined.apply(5)); // Результат: 16 ((5 + 3) * 2)
  ```

**`BiFunction<T, U, R>`**
- Как и `Function`, `BiFunction` поддерживает метод `andThen` для объединения с другой функцией:
  ```java
  BiFunction<Integer, Integer, Integer> sum = (a, b) -> a + b;
  Function<Integer, String> convertToString = x -> "Result: " + x;
  BiFunction<Integer, Integer, String> combined = sum.andThen(convertToString);
  System.out.println(combined.apply(2, 3)); // Результат: "Result: 5"
  ```

**`UnaryOperator<T>`**
- Это специализированный интерфейс, который наследуется от `Function<T, T>` и также поддерживает `andThen` и `compose`:
  ```java
  UnaryOperator<Integer> multiplyBy2 = x -> x * 2;
  UnaryOperator<Integer> add3 = x -> x + 3;
  UnaryOperator<Integer> combined = multiplyBy2.andThen(add3);
  System.out.println(combined.apply(5)); // Результат: 13
  ```

**`Predicate<T>`**
- Метод `and(Predicate<? super T> other)`, `or(Predicate<? super T> other)`, и `negate()` позволяют объединять несколько предикатов:
  ```java
  Predicate<String> isNotEmpty = s -> !s.isEmpty();
  Predicate<String> containsA = s -> s.contains("A");
  Predicate<String> combined = isNotEmpty.and(containsA);
  System.out.println(combined.test("Apple")); // Результат: true
  ```

**`Consumer<T>`**
- Метод `andThen(Consumer<? super T> after)` позволяет объединять несколько операций, выполняя их последовательно:
  ```java
  Consumer<String> print = System.out::println;
  Consumer<String> printLength = s -> System.out.println(s.length());
  Consumer<String> combined = print.andThen(printLength);
  combined.accept("Hello"); // Выводит "Hello" и "5"
  ```

Эти интерфейсы поддерживают композицию и объединение функций, что делает их полезными для построения цепочек операций и работы с функциями высшего порядка.

`Supplier<T>` — это функциональный интерфейс, который не принимает входных параметров и возвращает значение типа `T`. Он не поддерживает функции высшего порядка в том же смысле, как, например, `Function`, `Predicate`, или `Consumer`, поскольку у него нет методов для объединения или компоновки с другими `Supplier`. Метод `get()` у `Supplier` просто возвращает результат, и нет встроенных методов для комбинирования с другим `Supplier`. Однако, `Supplier` можно использовать в качестве функции высшего порядка, если его передавать в другие методы или возвращать из них. Например, `Supplier` может быть аргументом для метода, который использует или возвращает другие `Supplier`. Но сам по себе он не обладает методами, такими как `andThen`, `compose` или `or`.

### 6. Что такое ссылки на методы?

Это вызов метода через двойное двоеточие - оператор (::), а не через точку (.).

### 7. Что такое ссылки на конструкторы?

Ссылки на конструкторы в Java создаются аналогично тому, как создаются ссылки на методы.

  ```java
ИмяКласса::new
  ```

Такую ссылку можно присвоить любой ссылке на функциональный интерфейс, в котором определен метод, совместимый с этим конструктором. Обратите внимание, что "ИмяКласса" не может быть интерфейсом или абстрактным классом.

### 8. Расскажите о зоне видимости переменных в lambda-выражениях?

Переменная, объявленная внутри лямбда-выражения, доступна только в самом лямбда-выражении. Переменная, объявленная вне лямбда-выражения, доступна как внутри лямбда-выражения, так и вне его. Способ использования переменных в лямбда-выражениях зависит от того, где определена переменная - на уровне класса или на уровне метода, в котором находится лямбда-выражение. Статические переменные и переменные экземпляра (поля класса), могут быть использованы и изменены в лямбда-выражении. Если в лямбда-выражении используется локальная переменная, объявленная на уровне класса вне этого выражения (то есть локальная переменная метода), то это называется захватом переменной. Захваченную переменную изменять нельзя, так как в лямбда-выражении захваченная переменная не может быть изменена после своей инициализации. Можно использовать либо переменные с модификатором final, либо effective final переменные. effective final - это обычные переменные, только которые больше не изменяются после своей инициализации. Если необходимо использовать в лямбда-выражении внешние переменные, которые были изменены после их первоначальной инициализации, то вместо них нужно использовать промежуточные переменные, специально объявленные и инициализированные нужным значением непосредственно перед лямбда-выражением

### 9. Как быть в ситуации, если внутри lambda-выражения операторы могут выкинуть исключение?

Лямбда-выражение может генерировать исключение. Если это исключение является проверяемым (Checked), то оно должно быть совместимо с исключениями, которые перечислены в выражении throws в объявлении абстрактного метода в функциональном интерфейсе, который эта лямбда реализует, иначе программа не скомпилируется.

Проверяемые исключения можно так же обработать в блоке try-catch похожим образом. Для упрощения кода можно создать отдельный функциональный интерфейс и вывести обработку исключения в отдельный метод.

### 10. Что такое Stream API?

Интерфейс java.util.Stream позволяет гибко работать с коллекциями.

Stream API работает совместно с лямбда-выражениями. Stream API - это поток данных.

Каждый элемент коллекции проходит 3 стадии: фильтрация, преобразование и упрощение или аккумуляция.

Каждая стадия может использоваться отдельно или совместно.

### 11. Расскажите, какие шаблоны проектирования используются внутри Stream API? (Builder, Strategy, Decorator, Factory Method, Pipeline).

Внутри `Stream` API используются несколько шаблонов проектирования, которые обеспечивают гибкость, читаемость и эффективность работы с потоками данных. Основные из них - Builder, Strategy, Decorator, Factory Method, Pipeline. Эти шаблоны проектирования делают `Stream` API мощным инструментом для работы с потоками данных, позволяя гибко управлять операциями над данными и оптимизировать их обработку.

### 12. Объясните, где они используются в Stream API.

1. **Шаблон проектирования *Builder* (строитель)**
- В `Stream` API, создание цепочек операций над потоком данных напоминает шаблон `Builder`. Мы создаем поток данных, затем добавляем операции (такие как `filter`, `map`, `sorted` и т.д.), и только после вызова терминальных операций (`collect`, `forEach`, `reduce`) запускаем выполнение этих операций.
- Такой подход позволяет «строить» цепочку действий над потоком данных, что делает код более читаемым и наглядным.
- Например:
  ```java
  List<Integer> numbers = List.of(1, 2, 3, 4, 5);
  List<Integer> result = numbers.stream()
                                .filter(n -> n % 2 == 0)
                                .map(n -> n * n)
                                .collect(Collectors.toList());
  ```

2. **Шаблон проектирования *Decorator* (декоратор)**
- Потоки в `Stream` API являются неизменяемыми, и каждый вызов промежуточной операции, такой как `filter`, `map` или `sorted`, создает новый поток с добавленной функциональностью, что похоже на шаблон `Decorator`.
- Каждая операция над потоком добавляет новое поведение (например, фильтрацию, преобразование элементов) к существующему потоку, не изменяя исходный.
- Это позволяет комбинировать разные операции над потоком, не изменяя основной поток и добавляя новый функционал поверх уже существующего.

3. **Шаблон проектирования *Iterator* (итератор)**
- `Stream` сам по себе не является `Iterator`, но его концепция построена на принципе итерации элементов в потоке данных.
- `Stream` API позволяет последовательно обрабатывать каждый элемент, подобно тому, как это делает классический `Iterator`, но на более высоком уровне абстракции.
- Поток данных создается и затем поэтапно обрабатывается каждой операцией, при этом элементы могут не храниться в памяти все одновременно, что улучшает работу с большими наборами данных.

4. **Шаблон проектирования *Strategy* (стратегия)**
- В `Stream` API используются лямбда-выражения или ссылки на методы для определения поведения отдельных операций, таких как фильтрация, маппинг и сортировка.
- Например, `filter` принимает стратегию (предикат), которая определяет, какие элементы должны быть отфильтрованы.
- Таким образом, мы можем легко передавать различное поведение для операций в виде лямбда-выражений:
  ```java
  Stream.of("a", "ab", "abc")
        .filter(s -> s.length() > 1)
        .forEach(System.out::println);
  ```

5. **Шаблон проектирования *Factory* (фабрика)**
- Потоки создаются с помощью статических методов `Stream` — например, `Stream.of()`, `Collection.stream()`, `Arrays.stream()` и т.д. Это реализация шаблона `Factory`.
- Фабричные методы скрывают детали создания потоков и позволяют пользователю создавать их разными способами, например, из коллекций, массивов или даже бесконечных потоков (`Stream.generate`, `Stream.iterate`).

6. **Шаблон проектирования *Chain of Responsibility* (цепочка обязанностей)**
- Цепочки методов в `Stream` API напоминают `Chain of Responsibility`, так как каждый элемент потока передается по цепочке промежуточных операций.
- Например, когда поток проходит через `filter`, затем `map`, и затем `forEach`, каждый элемент обрабатывается каждой из этих операций поочередно.
- Если операция `filter` отфильтровала элемент, то этот элемент уже не передается дальше по цепочке.

7. **Шаблон проектирования *Lazy Evaluation* (ленивое вычисление)**
- Это не совсем классический шаблон проектирования, но `Stream` API применяет его концепцию. Промежуточные операции, такие как `filter` и `map`, не выполняются до тех пор, пока не вызвана терминальная операция (`collect`, `reduce`, `forEach`).
- Это позволяет избежать лишних вычислений и обрабатывать элементы потока только по мере необходимости, что повышает производительность при работе с большими данными.

### 13. Что такое конвейерные и терминальные операции?

В Java Stream API представлены 2 типа методов:

- Промежуточные (конвейерные). Преобразовывают элементы потока, возвращая новый преобразованный поток. Методов данного типа может быть сколько угодно в цепочке преобразований элементов потока. Данные методы ленивы, то есть отрабатывают только когда для потока вызван конечный метод. Промежуточных методов у потока может и не быть. Промежуточные операции не выполняются без конечных! Промежуточные методы ленивы - они начинают вычисляться только когда начнется терминальная операция, то есть вычисление происходит только тогда, когда оно нужно.

- Конечные (терминальные). Метод данного типа всегда один, располагается в конце цепочки промежуточных методов (если они есть). Данный метод возвращает другой тип объекта (например, Optional, коллекцию и т.д.). То есть конечный метод собирает результаты обработки элементов потока и возвращает единый результат. Конечный метод для завершения потока обязателен.

### 14. Перечислите конвейерные (промежуточные) методы Stream API.

Промежуточные (конвейерные) методы:

- filter() - фильтрует элементы потока, возвращая только элементы, удовлетворяющие условию.

- map() - преобразует каждый элемент потока.

- mapToInt() - тот же map(), но возвращает поток примитивов int (также есть соответствующие mapToDouble() и mapToLong()).

- flatMap() - трансформирует каждый объект потока в поток других объектов, то есть все элементы коллекции коллекций или потока потоков трансформирует в единый поток. (также поддерживает возврат потоков примитивов с помощью методов flatMapToInt(), flatMapToDouble(), flatMapToLong()). Может преобразовывать элементы, применяя указанную функцию к каждому элементу.

- peek() - применяет функцию Consumer к каждому элементу потока.

- sorted() - сортирует элементы потока по возрастанию. Возможна сортировка по убыванию при передаче соответствующего компаратора.

-  skip() - пропускает указанное число элементов с начала потока.

- limit() - делает выборку первых элементов из родного потока в указанном количестве (отбирает элементы из потока, пока не достигнет указанного количества).

- distinct() - убирает дубликаты из потока.

- mapToObj() - трансформирует числовой поток в объектный.

### 15. Перечислите терминальные методы Stream API.

Конечные (терминальные) методы:

- forEach() - применяет функцию к каждому элементу потока.

- collect() - собирает все элементы потока в структуру данных.

- toArray() - собирает элементы потока в массив.

- count() - возвращает количество элементов в потоке.

- min() - возвращает минимальный элемент (условие передается в компараторе).

- max() - возвращает максимальный элемент (условие передается в компараторе).

- sum() - возвращает сумму всех элементов потока (только для числовых потоков).

- average() - возвращает среднее арифметическое всех элементов потока (только для числовых потоков).

- allMatch() - возвращает true, если все элементы удовлетворяют условию.

- noneMatch() - возвращает true, если все элементы не удовлетворяют условию.

Конечные (терминальные) методы, делающие обход потока только до нахождения элемента, удовлетворяющего условию:

- findFirst() - возвращает Optional с первым элементом потока (если он есть), иначе возвращает пустой Optional.

- findAny() - возвращает Optional со случайным элементом потока.

- anyMatch() - возвращает true, если хотя бы один элемент удовлетворяет условию.

### 16. Что такое отложенное выполнение lamdba?

Лямбда обладает свойством отложенного выполнения (deferred execution). У отложенного выполнения есть и "брат-близнец" - ленивая загрузка (lazy load).

Разница между ними в том, что отложенное исполнение начинает вычисление только тогда, когда пользователь запросил его результат, а ленивая загрузка подразумевает, что работа не будет производиться, пока она не понадобится.

Ленивая загрузка упоминается чаще в контексте загрузки элементов или объектов. Например, загрузка классов ссылочных полей загружаемого класса будет выполняться только тогда, когда к ним произойдёт явное обращение в программе. (Другой способ - это eager (энергичная) загрузка, когда вся последовательность элементов загружается сразу, а не только когда элемент необходим).

Отложенное выполнение более точно описывает процесс, происходящий в лямбде - код внутри неё начнет выполняться только тогда, когда произойдёт обращение за его результатом.

### 17. Что делает метод filter()?

filter() фильтрует элементы потока, возвращая только элементы, удовлетворяющие условию.

filter() принимает объект Predicate<Task>, если метод возвращает true, то элемент передается дальше.

### 18. Что делает метод map()?

map() преобразует каждый элемент потока.

### 19. Что делает метод flatMap()?

flatMap() - трансформирует каждый объект потока в поток других объектов, то есть все элементы коллекции коллекций или потока потоков трансформирует в единый поток. (также поддерживает возврат потоков примитивов с помощью методов flatMapToInt(), flatMapToDouble(), flatMapToLong()). Может преобразовывать элементы, применяя указанную функцию к каждому элементу.

mapToInt() - тот же map(), но возвращает поток примитивов int (также есть соответствующие mapToDouble() и mapToLong()).

### 20. Что делает метод collect?

collect() - собирает все элементы потока в структуру данных.

### 21. Что делает метод findFirst?

findFirst() - возвращает Optional с первым элементом потока (если он есть), иначе возвращает пустой Optional.

findFirst() - терминальная операция, получает первый элемент из потока.

### 22. Что делает метод reduce?

Метод reduce() применяет операцию ко всем элементам потока. В этом методе понятие агрегации обобщается, так как в нем можно самостоятельно задать критерий для получения значения. Метод является терминальным. Метод reduce() имеет 3 формы.

- reduce(BinaryOperator<T> accumulator) -  стандартный вариант метода reduce(). Применяет функцию BinaryOperator ко всем элементам потока. Метод работает так: Например, есть поток из чисел 1, 2, 3, 4. Нам нужно сложить все эти числа. Метод reduce() сначала сложит первые 2 элемента - (1 + 2 = 3), далее к этому результату будет прибавлен следующий элемент (3 + 3 = 6) и так далее, пока не будут сложены все элементы потока. В результате весь поток будет сведен к единому результату. В нашем случае это сумма всех чисел. Данная форма метода возвращает Optional<T> (значение в обертке).

- reduce(T identity, BinaryOperator<T> accumulator) - эта версия метода похожа на предыдущую, только в BinaryOperator первым параметром будет identity - значение, к которому будет применяться второй элемент в функции. Если поток будет пуст, то identity останется значением по умолчанию. Данная форма метода возвращает само значение (T).

- reduce (U identity, BiFunction<U, ? super T,U> accumulator, BinaryOperator<U> combiner) - расширенная версия второй формы. accumulator здесь позволяет выполнить промежуточное действие над элементами потока, после чего к ним будет применена агрегатная операция combiner. Данная форма метода аналогична комбинации методов map() и второй формы reduce(), то есть если нам нужно вернуть тип данных, отличный от входящего, то нужно использовать эту версию метода reduce().

### 23. Что делают методы min и max?

Агрегатный терминальный метод min() возвращает минимальный элемент потока. Наименьший элемент определяется с помощью компаратора, который передается в параметр метода.

Метод max() возвращает максимальный элемент потока. Наибольший элемент определяется с помощью компаратора, который передается в параметр метода. Работает аналогично методу min().

### 24. Что делают методы count, sum, average?

count() - возвращает число элементов в потоке.
sum() - суммирует все элементы потока. Применяется только к числовым потокам.
average() - возвращает среднее арифметическое всех элементов потока. Применяется только к числовым потокам. Метод возвращает OptionalDouble (значение double, обернутое в Optional).

### 25. Что делают методы forEach и peek?

2 метода Stream API - forEach() и peek() - выполняют одну и ту же функцию - применяют действие ко всем элементам потока.

peek() - это промежуточная операция. Выполняет действие для каждого элемента потока, возвращая поток, состоящий из измененных элементов.

forEach() - это конечная операция. Выполняет действие для каждого элемента потока, завершая поток.

### 26. Что делают методы skip и limit?

В потоке нет индексов. Процесс работы потока - это последовательный перебор всех элементов указанной структуры данных с применением к ним промежуточных операций после того, как запущена конечная операция. Из-за отсутствия индексов мы не можем таким образом выборочно работать с элементами, но в Java Stream API существуют методы, которыми можно вручную ограничить диапазон перебираемых элементов - skip() и limit().

- skip(n) - промежуточная операция, пропускает первые n элементов с начала потока. То есть задает начальную границу диапазона перебираемых элементов. Параметром нельзя задать отрицательное число, а если заданное значение параметра равно или превышает число элементов в исходной структуре данных, то будет возвращен пустой поток. Важно! Числа, передающиеся в параметрах методов потоков, начинают отсчет с 1, так как исчисляется количество элементов. Не путать с индексом, где нумерация начинается с 0.

- limit(n) - промежуточная операция, возвращает новый поток, содержащий только первые n элементов из исходного потока, то есть задает конечную границу диапазона перебираемых элементов. Нельзя задать отрицательное значение параметра. Этот метод перебирает элементы, пока не накопит указанное количество элементов, после чего завершает перебор исходной структуры данных и возвращает новый поток из собранных элементов.

### 27. Что делают методы allMatch(), noneMatch() и anyMatch()?

Методы allMatch(), noneMatch() и anyMatch() проверяют элементы на соответствие заданному условию. Все эти методы принимают Predicate в качестве параметра (условия для проверки). Также все 3 метода являются конечными (терминальными).

- allMatch() - возвращает true, если все элементы потока удовлетворяют условию.
- 
- noneMatch() - возвращает true, если ни один из элементов потока не удовлетворяет условию.
- 
- anyMatch() -  возвращает true, если как минимум один из элементов потока удовлетворяет условию.

### 28. Что делают методы mapToInt(), flatMapToInt(), mapToObj()?

- mapToInt() - преобразовывает поток объектов в поток примитивных чисел типа int. Является промежуточной операцией. Применяется, если исходящий поток состоит из элементов-объектов, а в результате их обработки будет получен примитивный тип int. Метод mapToInt возвращает объект интерфейса IntStream (числовой поток), который в дальнейшем обрабатывается как поток примитивных чисел. Класс Stream помимо метода mapToInt() также имеет аналогичные ему методы mapToDouble() и mapToLong(), работающие с Double и Long типами соответственно.

- flatMapToInt() - трансформирует поток массивов в поток примитивных чисел int. Работает как mapToInt(), но возвращает не одно преобразованное в int значение, а поток примитивов int. Является промежуточной операцией. Класс Stream помимо метода flatMapToInt() также имеет аналогичные ему методы  flatMapToDouble() и flatMapToLong(), работающие с Double и Long типами соответственно.

- mapToObj() - преобразует поток примитивных чисел в поток объектов. Является промежуточной операцией.

### 29. Что такое числовой поток?

Числовой поток - это поток примитивов.

Чтобы обеспечить способ работы с тремя наиболее часто используемыми типами примитивов – int, long и double – библиотека java.util.stream включает три реализации стрима примитивов: public interface IntStream extends BaseStream<Integer,IntStream>, public interface LongStream extends BaseStream<Long,LongStream> и public interface DoubleStream extends BaseStream<Double,DoubleStream>.

### 30. Чем отличается Stream<Integer> от IntStream<int>?

Интерфейс IntStream описывает поток примитивных чисел int. То есть это такой же поток, как и Stream, только из int.

### 31. Что делает метод boxed?

Метод boxed() обертывает примитивные элементы потока.

### 32. Возможно ли прервать выполнение потока по аналогии с break?

Stream.takeWhile - этот метод позволяет получать поток данных до тех пор, пока условие истина.

### 33. Возможно ли пропустить элемент потока по аналогии с continue?

Stream.dropWhile - пропускает все элементы потока, пока они удовлетворяют условию. При обработке первого элемента, который не удовлетворяет условию, этот и все остальные элементы независимо от условия будут проходить дальше.

Stream.ofNullable - сделает из элемента поток, если элемент равен null, то элемент пропускается. Этот метод позволяет фильтровать поток с null элементами.

### 34. Что такое Optional?

В JDK 1.8 появился класс java.util.Optional. Задача класса java.util.Optional - это устранить появление в программе NullPointerException. Класс java.util.Optional - это обертка на null.

### 35. Перечислите методы Optional.

У класса java.util.Optional есть два метода.

Чтобы обернуть null, используется метод Optional.empty().

Чтобы обернуть не null, используется метод Optional.of().

Объект Optional предупреждает программиста, что метод может вернуть null и нужно добавить проверку, что объект не null-ссылка.

Чтобы проверить, что объект не null, используйте if (optional.isPresent()) {}.

Чтобы получить значение этого объекта, используйте метод optional.get().

### 36. В чем разница между методами orElse() и orElseGet()?

`orElse()` и `orElseGet()` — это методы класса `Optional`, которые применяются для получения значения, когда `Optional` пуст, но они различаются по своему поведению и по тому, когда вычисляется значение по умолчанию. Рассмотрим их разницу подробнее:

**`orElse()`**
- **Сигнатура**: `T orElse(T other)`
- **Описание**: Этот метод принимает объект в качестве аргумента и возвращает его, если `Optional` пуст, или возвращает значение, которое хранится в `Optional`, если оно присутствует.
- **Когда вычисляется значение**: Аргумент метода `orElse()` всегда вычисляется, независимо от того, пустой `Optional` или нет.
- **Пример**:
  ```java
  Optional<String> optional = Optional.of("Hello");
  String result = optional.orElse(getDefault());
  ```
  В этом примере метод `getDefault()` будет вызван даже тогда, когда `Optional` содержит значение `"Hello"`, хотя оно и не понадобится. Это может привести к ненужным вычислительным затратам.

**`orElseGet()`**
- **Сигнатура**: `T orElseGet(Supplier<? extends T> supplier)`
- **Описание**: Этот метод принимает `Supplier` в качестве аргумента. Если `Optional` содержит значение, оно будет возвращено. Если же `Optional` пуст, вызывается `Supplier`, и его результат используется в качестве значения по умолчанию.
- **Когда вычисляется значение**: Аргумент метода `orElseGet()` (то есть `Supplier`) будет вычислен только в случае, если `Optional` пуст.
- **Пример**:
  ```java
  Optional<String> optional = Optional.of("Hello");
  String result = optional.orElseGet(() -> getDefault());
  ```
  В этом примере метод `getDefault()` будет вызван только в том случае, если `optional` пуст. Если же `optional` содержит значение `"Hello"`, `getDefault()` вызван не будет.

**Итоговые различия**
- **Эффективность**: `orElseGet()` предпочтительнее, если вычисление значения по умолчанию — это дорогостоящая операция или требует времени, так как оно выполняется только тогда, когда это действительно необходимо.
- **Синтаксис**: `orElse()` требует просто значения по умолчанию, а `orElseGet()` — `Supplier`, то есть лямбду или ссылку на метод, который возвращает значение по умолчанию.
- **Использование**: `orElse()` удобнее, когда значение по умолчанию уже известно или легко доступно, тогда как `orElseGet()` лучше подходит, если значение по умолчанию нужно вычислить или если вычисление требует значительных ресурсов.

**Пример сравнения**
- **`orElse()` пример с ненужной нагрузкой**:
  ```java
  Optional<String> optional = Optional.of("value");
  String result = optional.orElse(computeExpensiveValue());
  ```
  Даже если `optional` содержит `"value"`, метод `computeExpensiveValue()` все равно будет вызван.

- **`orElseGet()` пример с ленивым вычислением**:
  ```java
  Optional<String> optional = Optional.of("value");
  String result = optional.orElseGet(() -> computeExpensiveValue());
  ```
  В этом случае метод `computeExpensiveValue()` вызовется только если `optional` окажется пустым.

Таким образом, **`orElseGet()` лучше использовать, когда значение по умолчанию требует ресурсов для вычисления**, а **`orElse()` — когда значение по умолчанию просто и уже доступно**.

### 37. Расскажите про фабричные методы List.of, Set.of, Map.of?

Это метод получения списка из набора элементов. Метод of() возвращает коллекцию, которую можно прочитать, но нельзя изменять или добавлять элементы.

### 38. Для чего используется var?

var - это ограниченный идентификатор, который упрощает создание переменных. Он имеет особое значение как тип объявления локальной переменной. var не является ключевым словом согласно спецификации языка. 

  ```java
var list = List.of(2, 4, 3, 4);
  ```

### 39. В каких случаях можно использовать var?

var может использоваться только в сочетании с данными. 