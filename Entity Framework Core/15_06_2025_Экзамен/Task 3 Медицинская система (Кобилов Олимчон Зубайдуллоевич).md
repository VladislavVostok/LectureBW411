
## Описание

Создайте консольное приложение "Медицинская система" на .NET 10 с использованием Entity Framework Core.

## Модели данных

- **Doctor** (Id, Name, Specialization, Phone, Email)
- **Patient** (Id, Name, DateOfBirth, Phone, Address, InsuranceNumber)
- **Appointment** (Id, PatientId, DoctorId, AppointmentDate, Diagnosis, Price)
- **Medication** (Id, Name, Price, QuantityInStock)
- **Prescription** (Id, AppointmentId, MedicationId, Dosage, Duration)
- **Department** (Id, Name, Floor, Building)
- **DoctorDepartment** (DoctorId, DepartmentId)

## Функциональность

1. **Миграции и связи**: 
   - One-to-Many: Doctor→Appointments, Patient→Appointments, Appointment→Prescriptions
   - Many-to-Many: Doctor↔Department
   - One-to-Many: Medication→Prescriptions

2. **Заполнение таблиц**: SeedData() с 4 отделениями, 6 врачами, 8 пациентами, 15 записями

3. **Контекст**: AppDbContext с Fluent API, Soft Delete для Patient

4. **Хранимые процедуры**:
   - `GetAppointmentsByDoctor` - записи врача
   - `GetAppointmentsByPatient` - записи пациента
   - `GetDoctorsByDepartment` - врачи отделения
   - `GetPatientAppointmentsWithPrescriptions` - полная история
   - `CreateAppointmentWithPrescription` - создание с транзакцией

5. **Пользовательские функции**:
   - `GetTotalRevenueByDoctor` - доход врача
   - `GetPatientAge` - возраст пациента
   - `GetMedicationsLowStock` - лекарства с низким остатком

6. **Транзакции**: 
   - Отмена приёма с возвратом
   - Обновление остатков лекарств

## Вывод (SELECT-запросы)

1. Все врачи с отделениями
2. Все пациенты с возрастом
3. Все приёмы с диагнозами и врачами
4. Статистика по специальностям
5. Доход каждого врача
6. Популярные лекарства
7. Загрузка врачей по дням
8. Пациенты без приёмов
