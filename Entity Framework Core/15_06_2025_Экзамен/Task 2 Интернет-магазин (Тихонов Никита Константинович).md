
## Описание

Создайте консольное приложение "Интернет-магазин" на .NET 10 с использованием Entity Framework Core.

## Модели данных

- **Product** (Id, Name, Price, StockQuantity, CategoryId, SupplierId)
- **Category** (Id, Name, ParentCategoryId)
- **Supplier** (Id, Name, ContactEmail, Phone)
- **Customer** (Id, Name, Email, Address, RegistrationDate)
- **Order** (Id, CustomerId, OrderDate, Status, TotalAmount)
- **OrderItem** (Id, OrderId, ProductId, Quantity, UnitPrice)
- **Review** (Id, ProductId, CustomerId, Rating, Comment, ReviewDate)

## Функциональность

1. **Миграции и связи**: 
   - One-to-Many: Category→Products, Supplier→Products, Customer→Orders, Order→OrderItems
   - Self-referencing: Category (ParentCategory)
   - One-to-Many: Product→Reviews, Customer→Reviews

2. **Заполнение таблиц**: SeedData() с 4 категориями, 3 поставщиками, 10 товарами, 5 покупателями, 8 заказами

3. **Контекст**: AppDbContext с Fluent API, Soft Delete для Customer, глобальные фильтры

4. **Хранимые процедуры**:
   - `GetProductsByCategory` - товары по категории
   - `GetOrdersByCustomer` - заказы покупателя
   - `GetTopRatedProducts` - топ по рейтингу
   - `GetCustomerOrderHistory` - полная история заказов
   - `CreateOrderWithItems` - создание заказа с транзакцией

5. **Пользовательские функции**:
   - `GetTotalSpentByCustomer` - общая сумма покупок
   - `GetAverageRating` - средний рейтинг товара
   - `GetProductsLowStock` - товары с низким остатком

6. **Транзакции**: 
   - Массовое обновление цен с транзакцией
   - Отмена заказа с транзакцией

## Вывод (множество SELECT-запросов)

1. Все товары с категориями и поставщиками
2. Все заказы с деталями (товары, количество, цены)
3. Топ-5 дорогих товаров
4. Товары с низким остатком
5. Покупатели с наибольшей суммой заказов
6. Средний рейтинг каждого товара
7. Заказы по статусам
8. Все отзывы с оценками
