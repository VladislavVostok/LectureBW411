# **Лекция: Структуры в C++ – Полный Разбор**

## **Введение**

Структуры (`struct`) в C++ – это мощный инструмент для объединения переменных различных типов в один логически связанный объект. Они позволяют организовывать данные, работать с указателями, использовать массивы, перегружать операторы и добавлять методы. Сегодня мы разберём все аспекты работы со структурами в C++, от базовых понятий до сложных возможностей.

---

## **1. Определение структуры**

Структура позволяет объединить несколько переменных в один логический блок.

```
#include <iostream>

struct Person {
    std::string name;
    int age;
};
```

### **Создание экземпляра структуры**

```
Person p1 = {"Alice", 25};
std::cout << p1.name << " is " << p1.age << " years old.\n";
```

---

## **2. Указатели на структуры**

Указатели позволяют работать со структурами через адреса, избегая копирования.

```
Person p2 = {"Bob", 30};
Person* ptr = &p2;

std::cout << ptr->name << " is " << ptr->age << " years old.\n";
```

Эквивалентная запись:

```
std::cout << (*ptr).name << " is " << (*ptr).age << " years old.\n";
```

### **Динамическое выделение памяти**

```
Person* p3 = new Person{"Charlie", 40};
std::cout << p3->name << " is " << p3->age << " years old.\n";
delete p3;  // Освобождение памяти
```

---

## **3. Функции и структуры**

Мы можем передавать структуры в функции по значению или по указателю.

```
void printPerson(const Person* p) {
    std::cout << p->name << " is " << p->age << " years old.\n";
}

int main() {
    Person p = {"Dave", 35};
    printPerson(&p);
    return 0;
}
```

---

## **4. Массивы структур**

Структуры могут быть организованы в массив.

```
Person people[] = {{"Eve", 28}, {"Frank", 33}};

std::cout << people[0].name << "\n";   // Eve
std::cout << people[1].name << "\n";   // Frank
```

Также можно работать через указатели:

```
Person* ptrArray = people;
std::cout << ptrArray->name << "\n";   // Eve
std::cout << (ptrArray + 1)->name << "\n";  // Frank
```

---

## **5. Методы внутри структуры**

C++ позволяет добавлять функции прямо в структуру.

```
struct Person {
    std::string name;
    int age;

    void introduce() const {
        std::cout << "Hi, my name is " << name << " and I am " << age << " years old.\n";
    }
};
```

Теперь можно вызывать этот метод:

```
Person p = {"Alice", 25};
p.introduce();
```

---

## **6. Перегрузка операторов**

C++ позволяет перегружать операторы, чтобы удобно работать со структурами.

### **Перегрузка оператора сложения (+)**

```
struct Person {
    std::string name;
    int age;

    Person operator+(int years) const {
        return Person{name, age + years};
    }
};
```

Теперь можно писать:

```
Person p = {"Bob", 30};
Person olderBob = p + 5;
std::cout << olderBob.name << " is " << olderBob.age << " years old.\n";
```

### **Перегрузка оператора вывода (<<)**

```
#include <iostream>

struct Person {
    std::string name;
    int age;

    friend std::ostream& operator<<(std::ostream& os, const Person& p) {
        os << p.name << ", Age: " << p.age;
        return os;
    }
};
```

Теперь можно просто писать:

```
Person p = {"Charlie", 40};
std::cout << p << std::endl;
```

---

## **7. Полный пример**

Этот код объединяет всё, что мы узнали, в одном примере.

```
#include <iostream>
#include <string>

struct Person {
    std::string name;
    int age;

    void introduce() const {
        std::cout << "Hi, my name is " << name << " and I am " << age << " years old.\n";
    }

    Person operator+(int years) const {
        return Person{name, age + years};
    }

    friend std::ostream& operator<<(std::ostream& os, const Person& p) {
        os << p.name << ", Age: " << p.age;
        return os;
    }
};

void printPeople(const Person people[], int size) {
    for (int i = 0; i < size; ++i) {
        std::cout << people[i] << std::endl;
    }
}

int main() {
    Person people[] = {
        {"Alice", 25},
        {"Bob", 30},
        {"Charlie", 35}
    };

    printPeople(people, 3);

    Person* ptr = people;
    std::cout << "First person via pointer: " << ptr->name << std::endl;
    std::cout << "Second person via pointer: " << (ptr + 1)->name << std::endl;

    people[0].introduce();

    Person olderBob = people[1] + 5;
    std::cout << "Older Bob: " << olderBob << std::endl;

    return 0;
}
```

---