
> Для краткости `var data = new[] { 1, 2, 3, 4, 5 };` используется без повторного объявления.

---

## 1. Where – фильтрация

**Method**  
```csharp
data.Where(n => n % 2 == 0);
```

**Query**  
```csharp
from n in data
where n % 2 == 0
select n;
```

---

## 2. Select – проекция

**Method**  
```csharp
data.Select(n => n * n);
```

**Query**  
```csharp
from n in data
select n * n;
```

---

## 3. SelectMany – «выпрямление» вложенных коллекций

**Method**  
```csharp
var nested = new[] { new[] { 1, 2 }, new[] { 3 } };
nested.SelectMany(a => a);   // 1,2,3
```

**Query**  
```csharp
from arr in nested
from n in arr
select n;
```

---

## 4. OrderBy / ThenBy – сортировка

**Method**  
```csharp
people.OrderBy(p => p.Name).ThenBy(p => p.Age);
```

**Query**  
```csharp
from p in people
orderby p.Name, p.Age
select p;
```

---

## 5. GroupBy – группировка

**Method**  
```csharp
data.GroupBy(n => n % 2 == 0 ? "even" : "odd");
```

**Query**  
```csharp
from n in data
group n by n % 2 == 0 ? "even" : "odd" into g
select new { Key = g.Key, Items = g };
```

---

## 6. Join – внутреннее соединение

**Method**  
```csharp
persons.Join(pets,
             p  => p.Id,
             pet => pet.OwnerId,
             (p, pet) => new { p.Name, Pet = pet.Name });
```

**Query**  
```csharp
from p in persons
join pet in pets on p.Id equals pet.OwnerId
select new { p.Name, Pet = pet.Name };
```

---

## 7. GroupJoin – соединение с группировкой

**Method**  
```csharp
persons.GroupJoin(pets,
                  p => p.Id,
                  pet => pet.OwnerId,
                  (p, grp) => new
                  {
                      Owner = p.Name,
                      Pets  = grp.Select(x => x.Name).ToList()
                  });
```

**Query**  
```csharp
from p in persons
join pet in pets on p.Id equals pet.OwnerId into grp
select new
{
    Owner = p.Name,
    Pets  = (from x in grp select x.Name).ToList()
};
```

---

## 8. All / Any – кванторы

**Method**  
```csharp
data.All(n => n > 0);
data.Any(n => n % 2 == 0);
```

**Query**  
```csharp
(from n in data where n <= 0 select n).Any() == !data.All(n => n > 0);
/* All/Any не имеют прямого аналога в query, поэтому используется комбинация */
```

---

## 9. Contains – проверка вхождения

**Method**  
```csharp
data.Contains(3);
```

**Query**  
```csharp
(from n in data where n == 3 select n).Any();
```

---

## 10. Count / Sum / Min / Max / Average

**Method**  
```csharp
data.Count(n => n > 2);
data.Sum();
data.Min();
data.Max();
data.Average();
```

**Query**  
```csharp
(from n in data where n > 2 select n).Count();
/* агрегаты вызываются на запросе, поэтому синтаксис одинаков */
```

---

## 11. Aggregate – накопление

**Method**  
```csharp
data.Aggregate((a, b) => a * b); // произведение
```

**Query**  
```csharp
/* отсутствует в query-синтаксисе → используем метод */
```

---

## 12. Distinct – уникальные

**Method**  
```csharp
new[] { 1, 2, 2, 3 }.Distinct();
```

**Query**  
```csharp
(from n in new[] { 1, 2, 2, 3 } select n).Distinct();
```

---

## 13. Union / Intersect / Except – операторы множеств

**Method**  
```csharp
var a = new[] { 1, 2 };
var b = new[] { 2, 3 };
a.Union(b);
a.Intersect(b);
a.Except(b);
```

**Query**  
```csharp
/* только через вызов метода у запроса */
```

---

## 14. Take / Skip – разбиение страниц

**Method**  
```csharp
data.Take(3);
data.Skip(2).Take(2); // page 2, size 2
```

**Query**  
```csharp
(from n in data select n).Take(3);
```

---

## 15. TakeWhile / SkipWhile

**Method**  
```csharp
data.TakeWhile(n => n < 4);
```

**Query**  
```csharp
(from n in data select n).TakeWhile(n => n < 4);
```

---

## 16. First / Last / Single (+ *OrDefault)

**Method**  
```csharp
data.First();
data.LastOrDefault();
data.Single(n => n == 3);
```

**Query**  
```csharp
(from n in data select n).First();
```

---

## 17. ElementAt

**Method**  
```csharp
data.ElementAt(2);
```

**Query**  
```csharp
(from n in data select n).ElementAt(2);
```

---

## 18. ToList / ToArray / ToDictionary / ToHashSet – материализация

**Method**  
```csharp
data.ToList();
data.ToDictionary(n => n);
```

**Query**  
```csharp
(from n in data select n).ToList();
```

---

## 19. Reverse

**Method**  
```csharp
data.Reverse();
```

**Query**  
```csharp
(from n in data select n).Reverse();
```

---

## 20. SequenceEqual – проверка последовательностей

**Method**  
```csharp
data.SequenceEqual(new[] { 1, 2, 3, 4, 5 });
```

**Query**  
```csharp
/* только через вызов метода */
```

---

## 21. Zip – «застёжка» двух коллекций

**Method**  
```csharp
var letters = new[] { "A", "B", "C" };
data.Zip(letters, (n, l) => $"{l}{n}");
```

**Query**  
```csharp
/* отсутствует в query → используем метод */
```

---

# Задачи средней сложности

---

### Задача 1  
**Дан массив целых. Найти три наибольших **различных** числа и вывести их в порядке убывания.**

**Method**  
```csharp
var top3 = data.Distinct()
               .OrderByDescending(x => x)
               .Take(3)
               .ToList();
```

**Query**  
```csharp
var top3 = (from n in data
            group n by n into g
            select g.Key into distinctNums
            orderby distinctNums descending
            select distinctNums).Take(3).ToList();
```

---

### Задача 2  
**Есть список студентов (`Name`, `GroupId`) и список групп (`GroupId`, `GroupName`).  
Вывести имя каждого студента и название его группы; пропустить студентов, чья группа не найдена.**

**Method**  
```csharp
students.Join(groups,
              s => s.GroupId,
              g => g.GroupId,
              (s, g) => new { s.Name, g.GroupName });
```

**Query**  
```csharp
from s in students
join g in groups on s.GroupId equals g.GroupId
select new { s.Name, g.GroupName };
```

---

### Задача 3  
**Дан `string[] words`. Получить строку, содержащую **все** уникальные символы, которые встречаются **хотя бы в трёх** разных словах, в алфавитном порядке.**

**Method**  
```csharp
string result = string.Concat(
        words.SelectMany(w => w.Distinct())
             .GroupBy(ch => ch)
             .Where(g => g.Select((_, i) => i).Distinct().Count() >= 3)
             .Select(g => g.Key)
             .OrderBy(ch => ch));
```

**Query**  
```csharp
var chars = from w in words
            from ch in w.Distinct()
            group ch by ch into g
            where (from w2 in words
                   where w2.Contains(g.Key)
                   select w2).Count() >= 3
            orderby g.Key
            select g.Key;
string result = string.Concat(chars);
```

---

### Задача 4  
**Дан `int[] numbers`. Найти **самую длинную строго возрастающую подпоследовательность**, начинающуюся с индекса 0 (если длины равны – первую появившуюся).**

**Method**  
```csharp
var longest = numbers
    .Scan(new List<int>(), (acc, n) =>
    {
        if (acc.Count == 0 || n > acc.Last()) acc.Add(n);
        else acc = new List<int> { n };
        return acc;
    })
    .OrderByDescending(lst => lst.Count)
    .FirstOrDefault();
```

*(метод `Scan` можно реализовать через агрегат или взять из пакета MoreLINQ; ниже версия без сторонних библиотек)*

```csharp
List<int> best = new(), cur = new();
foreach (var n in numbers)
{
    if (cur.Count == 0 || n > cur.Last()) cur.Add(n);
    else cur = new List<int> { n };
    if (cur.Count > best.Count) best = cur.ToList();
}
```

**Query**  
```csharp
/* алгоритм требует накопления состояния – проще решить методами */
```
