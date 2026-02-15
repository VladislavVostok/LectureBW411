
## Описание

Создайте консольное приложение "Транспортно-логистическая система" на .NET 10 с использованием Entity Framework Core.

## Модели данных

- **Vehicle** (Id, Model, LicensePlate, Capacity, DriverId, Status)
- **Driver** (Id, Name, LicenseNumber, Phone, HireDate)
- **Route** (Id, Name, StartPoint, EndPoint, Distance)
- **Trip** (Id, VehicleId, RouteId, DriverId, DepartureTime, ArrivalTime, CargoWeight, Status)
- **Cargo** (Id, Name, Weight, Volume, TripId, ClientId)
- **Client** (Id, Name, Email, Phone, Address)
- **Warehouse** (Id, Name, Address, Capacity)

## Функциональность

1. **Миграции и связи**: 
   - One-to-Many: Driver→Vehicles, Vehicle→Trips, Route→Trips, Client→Cargo
   - One-to-Many: Trip→Cargo

2. **Заполнение таблиц**: 5 водителей, 8 транспортных средств, 6 маршрутов, 15 рейсов, 10 клиентов

3. **Контекст**: AppDbContext с Fluent API

4. **Хранимые процедуры**:
   - `GetVehiclesByStatus` - транспорт по статусу
   - `GetTripsByDriver` - рейсы водителя
   - `GetTripsByRoute` - рейсы по маршруту
   - `GetDriverStatistics` - статистика водителя
   - `ScheduleTripWithTransaction` - планирование рейса с транзакцией

5. **Пользовательские функции**:
   - `GetTotalCargoWeight` - общий вес груза
   - `GetVehicleUtilization` - загрузка транспорта
   - `GetRouteRevenue` - доходность маршрута

6. **Транзакции**: 
   - Завершение рейса с обновлением статуса
   - Техническое обслуживание транспорта

## Вывод (SELECT-запросы)

1. Все транспортные средства с водителями
2. Активные рейсы с деталями
3. Статистика по маршрутам
4. Доходность водителей
5. Загрузка складов
6. Клиенты с грузами
7. Простой транспорта
8. Популярные маршруты
