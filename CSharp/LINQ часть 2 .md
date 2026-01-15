## Задание 4: Объединение коллекций (Join)

**Классы Order и Customer:**
```csharp
public class Order
{
    public int OrderId { get; set; }
    public string CustomerName { get; set; }
    public decimal TotalAmount { get; set; }
}

public class Customer
{
    public string Name { get; set; }
    public string Email { get; set; }
    public string City { get; set; }
}
```

**Исходные данные:**
```csharp
List<Order> orders = new List<Order>
{
    new Order { OrderId = 1, CustomerName = "Анна", TotalAmount = 15000 },
    new Order { OrderId = 2, CustomerName = "Иван", TotalAmount = 25000 },
    new Order { OrderId = 3, CustomerName = "Мария", TotalAmount = 18000 }
};

List<Customer> customers = new List<Customer>
{
    new Customer { Name = "Анна", Email = "anna@mail.ru", City = "Москва" },
    new Customer { Name = "Иван", Email = "ivan@mail.ru", City = "Санкт-Петербург" },
    new Customer { Name = "Мария", Email = "maria@mail.ru", City = "Москва" },
    new Customer { Name = "Петр", Email = "petr@mail.ru", City = "Казань" }
};
```

**Задачи:**
1. Объедините заказы с клиентами по имени
2. Для каждого заказа выведите: Имя клиента, Email, Город, Сумма заказа
3. Найдите общую сумму заказов для каждого города
4. Найдите клиентов, у которых нет заказов

---

## Задание 5: Комплексное задание - Анализ данных

**Класс Student:**
```csharp
public class Student
{
    public string Name { get; set; }
    public int GroupId { get; set; }
    public List<int> Grades { get; set; }
}
```

**Исходные данные:**
```csharp
List<Student> students = new List<Student>
{
    new Student { Name = "Алексей", GroupId = 1, Grades = new List<int> { 5, 4, 5, 3 } },
    new Student { Name = "Екатерина", GroupId = 1, Grades = new List<int> { 4, 4, 5, 5 } },
    new Student { Name = "Дмитрий", GroupId = 2, Grades = new List<int> { 3, 4, 3, 4 } },
    new Student { Name = "Светлана", GroupId = 2, Grades = new List<int> { 5, 5, 5, 5 } },
    new Student { Name = "Михаил", GroupId = 1, Grades = new List<int> { 4, 3, 4, 4 } }
};
```

**Задачи:**
1. Для каждого студента вычислите средний балл
2. Сгруппируйте студентов по группам и для каждой группы:
   - найдите средний балл группы
   - найдите студента с наивысшим средним баллом
3. Найдите всех студентов, у которых есть хотя бы одна оценка 5
4. Отсортируйте студентов по среднему баллу (по убыванию)
5. Создайте анонимный тип с данными: Имя, Группа, Средний балл, Количество оценок

---

## Методы LINQ для решения задач

### **Задание 4: Объединение коллекций**
- `Join` - объединение заказов с клиентами по имени
- `GroupJoin` - левое внешнее соединение
- `Select` - создание результирующих объектов с данными из обеих коллекций
- `GroupBy` - группировка по городам для подсчета сумм
- `Sum` - вычисление общей суммы заказов
- `Where` + `SelectMany` - поиск клиентов без заказов
- `DefaultIfEmpty` - для left join операций

### **Задание 5: Комплексный анализ данных**
- `Select` - вычисление среднего балла для каждого студента
- `Average` - вычисление среднего балла (для студента и группы)
- `GroupBy` - группировка студентов по группам
- `SelectMany` - работа с вложенными коллекциями оценок
- `OrderByDescending` - сортировка по среднему баллу
- `Max` - поиск максимального среднего балла в группе
- `Any` - проверка наличия хотя бы одной оценки "5"
- Анонимные типы - создание кастомных проекций
- Вложенные LINQ запросы - для сложных агрегаций

