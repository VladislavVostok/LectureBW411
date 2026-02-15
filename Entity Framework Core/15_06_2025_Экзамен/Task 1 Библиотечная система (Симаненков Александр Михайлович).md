
## Описание

Создайте консольное приложение "Библиотечная система" на .NET 10 с использованием Entity Framework Core.

## Модели данных

- **Author** (Id, Name, BirthDate)
- **Book** (Id, Title, PublishedYear, AuthorId)
- **Category** (Id, Name)
- **BookCategory** (BookId, CategoryId) - many-to-many
- **Reader** (Id, Name, Email, Phone, RegistrationDate)
- **Loan** (Id, ReaderId, BookId, LoanDate, ReturnDate)

## Функциональность

1. **Миграции и связи**: Настройте Code First миграции с One-to-Many (Author→Book, Reader→Loan) и Many-to-Many (Book↔Category) связями с каскадным удалением

2. **Заполнение таблиц**: Реализуйте SeedData() с проверкой существования данных, заполните минимум 3 авторов, 5 книг, 3 категории, 4 читателя

3. **Контекст**: Создайте AppDbContext с Fluent API для всех связей и глобальным фильтром Soft Delete 

4. **Хранимые процедуры**:
   - `GetBooksByAuthor` - получить книги по автору
   - `GetActiveLoansByReader` - активные займы читателя
   - `CreateLoanWithTransaction` - создание займа с транзакцией

5. **Пользовательские функции**:
   - Функция для подсчёта просроченных займов
   - Функция для расчёта штрафа за просрочку

6. **Транзакции**: Реализуйте метод продления займа с использованием явной транзакции

## Вывод

Выведите список всех книг с авторами и категориями, список читателей с их займами
