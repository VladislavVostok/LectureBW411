https://leetcode.com/problems/plus-one/description/

```c++
#include <iostream>

int* plusOne(int* digits, int size, int& newSize) {
    // Проходим с конца, чтобы обработать перенос (если есть)
    for (int i = size - 1; i >= 0; --i) {
        if (digits[i] < 9) {
            digits[i]++; // Если число меньше 9, просто увеличиваем
            newSize = size;
            return digits; // Возвращаем тот же массив
        }
        digits[i] = 0; // Если 9, устанавливаем в 0 и продолжаем
    }

    // Если все цифры были 9 (например, 999 -> 1000), создаем новый массив
    newSize = size + 1;
    int* newDigits = new int[newSize];
    newDigits[0] = 1;
    for (int i = 1; i < newSize; i++) {
        newDigits[i] = 0;
    }

    delete[] digits; // Освобождаем старый массив
    return newDigits; // Возвращаем новый массив
}

void printArray(int* arr, int size) {
    std::cout << "[ ";
    for (int i = 0; i < size; i++) {
        std::cout << arr[i] << " ";
    }
    std::cout << "]\n";
}

int main() {
    int test1[] = {1, 2, 3};
    int test2[] = {4, 3, 2, 1};
    int test3[] = {9};
    int test4[] = {9, 9, 9};
    
    int* digits1 = new int[3]{1, 2, 3};
    int* digits2 = new int[4]{4, 3, 2, 1};
    int* digits3 = new int[1]{9};
    int* digits4 = new int[3]{9, 9, 9};

    int newSize;
    int* result1 = plusOne(digits1, 3, newSize);
    printArray(result1, newSize);
    delete[] result1;

    int* result2 = plusOne(digits2, 4, newSize);
    printArray(result2, newSize);
    delete[] result2;

    int* result3 = plusOne(digits3, 1, newSize);
    printArray(result3, newSize);
    delete[] result3;

    int* result4 = plusOne(digits4, 3, newSize);
    printArray(result4, newSize);
    delete[] result4;

    return 0;
}
```

### **Разбор решения**:

1. **Используем `new` для динамического выделения памяти**:
    - Позволяет работать с массивами произвольного размера.
2. **Обрабатываем перенос (`carry`)**:
    - Если цифра `9`, меняем её на `0` и идём дальше.
    - Если цифра < 9, просто увеличиваем и выходим.
3. **Создаем новый массив при необходимости**:
    - Например, `999` превращается в `1000`, поэтому выделяется новый массив.
    - После этого освобождаем память (`delete[]`), чтобы избежать утечек.


### **Анализируем временную сложность последнего варианта:**

1. **Основной цикл (итерация с конца массива)**
    - В худшем случае (например, `[9, 9, 9]`) мы проходим все `n` элементов.
    - Это занимает **O(n)**.

2. **Создание нового массива (в случае полного переноса)**
    - Если весь массив состоял из `9`, создаётся новый массив размером `n + 1`.
    - Копирование элементов в новый массив также занимает **O(n)**.

3. **Удаление старого массива**
    - `delete[] digits` работает за **O(1)**, так как просто освобождает память.

### **Общий вывод**

- В **обычном случае** (без переноса всех цифр) мы изменяем одну цифру и завершаем за **O(1)**.
- В **худшем случае** (когда все цифры `9`) выполняем полный проход и выделяем новый массив, что занимает **O(n)**.

**Итоговая сложность**:

- **Лучший случай:** `O(1)` (например, `[1, 2, 3] → [1, 2, 4]`)
- **Худший случай:** `O(n)` (например, `[9, 9, 9] → [1, 0, 0, 0]`)

Это **оптимальное решение**, так как избежать прохода по массиву в худшем случае невозможно.


### **Можно ли это все ускорить???**

Ускорить решение в лучшем случае (без переноса) уже нельзя, так как оно выполняется за **O(1)**. Но можно оптимизировать некоторые моменты, чтобы уменьшить накладные расходы:

### Оптимизации:

1. **Использовать `realloc` вместо `new` + `delete[]`** (для `C++` это `std::unique_ptr` или `std::vector`).
    
    - В `C` можно использовать `realloc`, чтобы не создавать новый массив вручную.
    - В `C++` предпочтительно использовать `std::vector`, но раз требуется динамический массив, попробуем `realloc`.
2. **Минимизировать ненужные операции**:
    
    - Убираем `delete[] digits`, так как `realloc` сам перевыделяет память.
    - Избегаем лишних копирований при создании нового массива.


#### **Улучшенный код (на C-подобном стиле C++):**

```c++
#include <iostream>
#include <cstdlib> // Для realloc

int* plusOne(int* digits, int size, int& newSize) {
    // Проходим массив с конца
    for (int i = size - 1; i >= 0; --i) {
        if (digits[i] < 9) {
            digits[i]++; // Если цифра < 9, просто увеличиваем и выходим
            newSize = size;
            return digits;
        }
        digits[i] = 0; // Если 9, ставим 0 и идём дальше
    }

    // Если все цифры были 9, расширяем массив на 1 элемент
    newSize = size + 1;
    digits = (int*)realloc(digits, newSize * sizeof(int));
    digits[0] = 1;
    for (int i = 1; i < newSize; i++) {
        digits[i] = 0;
    }

    return digits; // Возвращаем новый массив
}

void printArray(int* arr, int size) {
    std::cout << "[ ";
    for (int i = 0; i < size; i++) {
        std::cout << arr[i] << " ";
    }
    std::cout << "]\n";
}

int main() {
    int* digits1 = (int*)malloc(3 * sizeof(int));
    digits1[0] = 1; digits1[1] = 2; digits1[2] = 3;
    
    int* digits2 = (int*)malloc(4 * sizeof(int));
    digits2[0] = 4; digits2[1] = 3; digits2[2] = 2; digits2[3] = 1;
    
    int* digits3 = (int*)malloc(1 * sizeof(int));
    digits3[0] = 9;
    
    int* digits4 = (int*)malloc(3 * sizeof(int));
    digits4[0] = 9; digits4[1] = 9; digits4[2] = 9;

    int newSize;
    int* result1 = plusOne(digits1, 3, newSize);
    printArray(result1, newSize);
    free(result1);

    int* result2 = plusOne(digits2, 4, newSize);
    printArray(result2, newSize);
    free(result2);

    int* result3 = plusOne(digits3, 1, newSize);
    printArray(result3, newSize);
    free(result3);

    int* result4 = plusOne(digits4, 3, newSize);
    printArray(result4, newSize);
    free(result4);

    return 0;
}

```

- **Используем `realloc`** для перевыделения памяти вместо создания нового массива и удаления старого. Это экономит время.
- **Отказались от `new` и `delete[]`** в пользу `malloc` / `free`, что ближе к низкоуровневому управлению памятью.
- **Не копируем массив вручную**, так как `realloc` уже копирует старые данные в новую область памяти.

### **Скоростные преимущества:**

- В случае полного переноса (`999 → 1000`) **`realloc` быстрее, чем `new` + `delete[]`**, так как:
    - Если хватает памяти, `realloc` просто расширит существующий блок без копирования.
    - В худшем случае копирование займёт **O(n)** (как и раньше), но средний случай стал быстрее.
- В случае простого увеличения (`123 → 124`) работает за **O(1)**.

### **Вывод**

- **Лучший случай:** `O(1)`
- **Худший случай:** `O(n)`, но быстрее за счёт `realloc`
- **Средний случай** быстрее за счёт экономии на выделении памяти.


### **Без выделения памяти на прямую через тип vector**


```c++
#include <vector>
#include <iostream>

std::vector<int> plusOne(std::vector<int>& digits) {
    int n = digits.size();
    
    // Проходим массив с конца (начиная с младшего разряда)
    for (int i = n - 1; i >= 0; --i) {
        if (digits[i] < 9) {
            // Если текущая цифра меньше 9, просто увеличиваем её и завершаем
            digits[i]++;
            return digits;
        }
        // Если цифра 9, устанавливаем её в 0 и переходим к следующей
        digits[i] = 0;
    }

    // Если все цифры были 9, добавляем 1 в начало массива
    digits.insert(digits.begin(), 1);
    return digits;
}

int main() {
    std::vector<std::vector<int>> test_cases = {
        {1, 2, 3},
        {4, 3, 2, 1},
        {9},
        {9, 9, 9},
        {0},
        {8, 9, 9, 9}
    };

    for (auto& digits : test_cases) {
        std::vector<int> result = plusOne(digits);
        std::cout << "[ ";
        for (int num : result) {
            std::cout << num << " ";
        }
        std::cout << "]\n";
    }

    return 0;
}

```