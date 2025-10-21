# JavaLab2 - Отчёт по лабораторной работе №2

<h2> Общая информация</h2>
<p><strong>Автор:</strong> Конов Михаил ПР-7<br>
<strong>Предмет:</strong> Программирование на Java<br>
<strong>Лабораторная работа:</strong> №2<br>
<strong>Вариант:</strong> 2</p>

<hr>

<h2> Задание 1. Базовые сущности</h2>

<h3> Задача 2,3. Человек и Имена</h3>

<p><strong>Реализованные классы:</strong></p>
<ul>
<li><code>Person</code> - сущность Человек</li>
<li><code>Name</code> - сущность Имя</li>
</ul>

<p><strong>Класс Person:</strong></p>
```java
public class Person {
    private Name name;
    private int height;
    
    // Конструкторы
    public Person(Name name, int height) {
        this.name = name;
        this.height = height;
    }
    
    public Person(String firstName, int height) {
        this(new Name(firstName), height);
    }
    
    // Геттеры и сеттеры
    public Name getName() { return name; }
    public int getHeight() { return height; }
    
    @Override
    public String toString() {
        return "Имя: " + name.toString() + ", рост: " + height + " см";
    }
}
<p><strong>Класс Name:</strong></p> ```java public class Name { private String lastName; private String firstName; private String middleName;
text
// Конструкторы для разных комбинаций параметров
public Name(String firstName) {
    this(null, firstName, null);
}

public Name(String lastName, String firstName) {
    this(lastName, firstName, null);
}

public Name(String lastName, String firstName, String middleName) {
    this.lastName = lastName;
    this.firstName = firstName;
    this.middleName = middleName;
}

@Override
public String toString() {
    StringBuilder result = new StringBuilder();
    if (lastName != null) result.append(lastName).append(" ");
    if (firstName != null) result.append(firstName).append(" ");
    if (middleName != null) result.append(middleName);
    return result.toString().trim();
}
}

text

<p><strong>Созданные объекты:</strong></p>
<ul>
<li>Человек с Именем "Клеопатра" и ростом 152</li>
<li>Человек с Именем "Пушкин Александр Сергеевич" и ростом 167</li>
<li>Человек с Именем "Маяковский Владимир" и ростом 189</li>
</ul>

<hr>

<h2> Задание 2. Композиция объектов</h2>

<h3> Задача 2. Человек с именем</h3>

<p><strong>Объединение сущностей:</strong></p>
<ul>
<li>Класс <code>Person</code> теперь использует <code>Name</code> вместо строки</li>
<li>Реализована проверка ввода данных</li>
<li>Добавлены методы для интерактивного создания объектов</li>
</ul>

<p><strong>Метод создания человека из ввода:</strong></p>
```java
public static Person createFromInput(Scanner scanner) {
    Name name = Name.createFromInput(scanner);
    int height = Validation.getInputInt(scanner, "Введите рост (см): ", 1, 300);
    return new Person(name, height);
}
<p><strong>Результат объединения:</strong></p> <ul> <li>Человек с Именем Клеопатра и ростом 152</li> <li>Человек с Именем Пушкин Александр Сергеевич и ростом 167</li> <li>Человек с Именем Маяковский Владимир и ростом 189</li> </ul><h3> Задача 3. Человек с родителем</h3><p><strong>Расширенный класс Person:</strong></p> ```java public class Person { private Name name; private int height; private Person father; // Добавлен отец
text
// Конструкторы
public Person(Name name, int height, Person father) {
    this.name = name;
    this.height = height;
    this.father = father;
    inheritFromFather();
}

// Наследование фамилии и отчества от отца
private void inheritFromFather() {
    if (father != null) {
        Name fatherName = father.getName();
        // Наследование фамилии
        if (!name.hasLastName() && fatherName.hasLastName()) {
            name.setLastName(fatherName.getLastName());
        }
        // Наследование отчества
        if (!name.hasMiddleName() && fatherName.hasFirstName()) {
            name.setMiddleName(fatherName.getFirstName() + "ович");
        }
    }
}
}

text

<p><strong>Созданные объекты:</strong></p>
<ul>
<li>Чудов Иван (без отца)</li>
<li>Чудов Пётр (отец - Иван)</li>
<li>Чудов Борис (отец - Пётр)</li>
</ul>

<p><strong>Автоматическое наследование:</strong></p>
<ul>
<li>Пётр наследует фамилию "Чудов" от Ивана</li>
<li>Борис наследует фамилию "Чудов" и отчество "Петрович" от Петра</li>
</ul>

<hr>

<h2> Задание 3. Коллекции и агрегация</h2>

<h3> Задача 3. Города</h3>

<p><strong>Класс City:</strong></p>
```java
public class City {
    private String name;
    private Map<City, Integer> routes; // Город -> стоимость поездки
    
    public City(String name) {
        this.name = name;
        this.routes = new HashMap<>();
    }
    
    // Добавление пути к другому городу
    public void addRoute(City city, int cost) {
        routes.put(city, cost);
    }
    
    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder();
        sb.append("Город: ").append(name);
        if (!routes.isEmpty()) {
            sb.append("\nСвязанные города:");
            for (Map.Entry<City, Integer> entry : routes.entrySet()) {
                sb.append("\n  - ").append(entry.getKey().getName())
                  .append(": ").append(entry.getValue());
            }
        }
        return sb.toString();
    }
}
<p><strong>Созданная схема городов:</strong></p> ``` Город: A Связанные города: - C: 10 - B: 5
Город: B
Связанные города:

A: 5

E: 8

D: 3

Город: C
Связанные города:

A: 10

E: 2

Город: D
Связанные города:

F: 7

B: 3

Город: E
Связанные города:

F: 4

C: 2

B: 8

Город: F
Связанные города:

E: 4

D: 7

text

<hr>

<h2> Задание 4. Улучшенные сущности</h2>

<h3> Задача 8. Создаем Города</h3>

<p><strong>Улучшенный класс City:</strong></p>
```java
public class City {
    private String name;
    private Map<City, Integer> routes;
    
    /* Новое требование: Город можно создать указав только название */
    public City(String name) {
        setName(name);
        this.routes = new HashMap<>();
    }
    
    /* Новое требование: Город можно создать указав название и набор связанных городов */
    public City(String name, Map<City, Integer> routes) {
        setName(name);
        this.routes = new HashMap<>(routes);
    }
    
    // Валидация названия города
    public void setName(String name) {
        if (Validation.isValidCityName(name)) {
            this.name = name.toUpperCase();
        } else {
            System.out.println("Ошибка: название должно быть одной заглавной латинской буквой");
            this.name = "A";
        }
    }
}
<p><strong>Проверки ввода:</strong></p> <ul> <li>Название должно быть одной заглавной латинской буквой</li> <li>Название должно быть уникальным среди существующих городов</li> <li>Предотвращение создания пути к самому себе</li> </ul><hr><h2> Задание 5. Функциональность объектов</h2><h3> Задача 5. Дроби</h3><p><strong>Класс Fraction:</strong></p> ```java public class Fraction { private int numerator; // числитель private int denominator; // знаменатель
text
public Fraction(int numerator, int denominator) {
    if (denominator == 0) throw new IllegalArgumentException("Знаменатель не может быть нулем");
    this.numerator = numerator;
    this.denominator = denominator;
    simplify();
}

// Операции с дробями
public Fraction add(Fraction other) {
    int newNum = this.numerator * other.denominator + other.numerator * this.denominator;
    int newDen = this.denominator * other.denominator;
    return new Fraction(newNum, newDen);
}

public Fraction subtract(Fraction other) {
    int newNum = this.numerator * other.denominator - other.numerator * this.denominator;
    int newDen = this.denominator * other.denominator;
    return new Fraction(newNum, newDen);
}

public Fraction multiply(Fraction other) {
    return new Fraction(this.numerator * other.numerator, 
                       this.denominator * other.denominator);
}

public Fraction divide(Fraction other) {
    return new Fraction(this.numerator * other.denominator, 
                       this.denominator * other.numerator);
}

@Override
public String toString() {
    return numerator + "/" + denominator;
}
}

text

<p><strong>Примеры операций:</strong></p>
<ul>
<li>1/3 + 2/3 = 3/3 = 1/1</li>
<li>3/4 - 1/2 = 1/4</li>
<li>1/3 * 2/3 = 2/9</li>
<li>3/4 / 1/2 = 6/4 = 3/2</li>
</ul>

<p><strong>Сложное выражение:</strong></p>
<ul>
<li>f1.add(f2).divide(f3).subtract(5) = -4 11/12</li>
</ul>

<hr>

<h2> Главный класс программы</h2>

<p><strong>Структура меню:</strong></p>
```java
public class Main {
    private static List<Person> people = new ArrayList<>();
    private static List<Name> names = new ArrayList<>();
    private static List<City> cities = new ArrayList<>();
    private static List<Fraction> fractions = new ArrayList<>();
    
    public static void main(String[] args) {
        // Инициализация данных
        initializeDefaultData();
        
        // Основное меню с разделами:
        // 1. Работа с людьми
        // 2. Работа с именами  
        // 3. Работа с городами
        // 4. Работа с дробями
        // 5. Выход
    }
}
<p><strong>Особенности реализации:</strong></p> <ul> <li>Дружественный интерфейс с проверкой ввода</li> <li>Возможность интерактивного создания объектов</li> <li>Демонстрация всех требуемых операций</li> <li>Обработка ошибок и валидация данных</li> </ul><hr><h2> Вывод</h2><p>В ходе лабораторной работы успешно реализованы все требуемые сущности согласно варианту 2:</p> <ol> <li>Создана система классов для работы с людьми, именами и наследованием</li> <li>Реализована схема городов с путями между ними</li> <li>Разработан класс для работы с дробями и математическими операциями</li> <li>Обеспечен дружественный интерфейс с проверкой ввода данных</li> <li>Все классы содержат необходимые свойства, конструкторы и метод toString()</li> </ol><p>Программа демонстрирует принципы объектно-ориентированного программирования: инкапсуляцию, наследование, полиморфизм и обеспечивает удобное взаимодействие с пользователем через консольное меню.</p> ```
