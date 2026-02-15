
## Описание

Создайте консольное приложение "Университетская система" на .NET 10 с использованием Entity Framework Core.

## Модели данных

- **Student** (Id, Name, DateOfBirth, GroupId, Email, EnrollmentYear)
- **Group** (Id, Name, FacultyId, Year)
- **Faculty** (Id, Name, DeanName, Building)
- **Subject** (Id, Name, Hours, TeacherId)
- **Teacher** (Id, Name, DepartmentId, Position, Email)
- **Department** (Id, Name, FacultyId)
- **Grade** (Id, StudentId, SubjectId, GradeValue, GradeDate)
- **SubjectTeacher** (SubjectId, TeacherId)

## Функциональность

1. **Миграции и связи**: 
   - One-to-Many: Faculty→Departments→Teachers, Group→Students, Teacher→Subjects
   - One-to-Many: Subject→Grades, Student→Grades
   - Many-to-Many: Subject↔Teacher

2. **Заполнение таблиц**: 3 факультета, 5 кафедр, 8 преподавателей, 6 групп, 20 студентов, 10 предметов, оценки

3. **Контекст**: AppDbContext с Fluent API

4. **Хранимые процедуры**:
   - `GetStudentsByGroup` - студенты группы
   - `GetGradesByStudent` - оценки студента
   - `GetSubjectTeachers` - преподаватели предмета
   - `GetGroupPerformance` - успеваемость группы
   - `AddGradeWithTransaction` - добавление оценки с транзакцией

5. **Пользовательские функции**:
   - `GetStudentGPA` - средний балл студента
   - `GetTeacherWorkload` - нагрузка преподавателя
   - `GetTopStudents` - лучшие студенты

6. **Транзакции**: 
   - Перевод студента в другую группу
   - Массовое обновление оценок

## Вывод (SELECT-запросы)

1. Все студенты с группами и факультетами
2. Средний балл по каждому предмету
3. Рейтинг преподавателей по нагрузке
4. Успеваемость по группам
5. Топ-10 лучших студентов
6. Предметы с оценками
7. Статистика оценок (5,4,3,2,1)
8. Кафедры с преподавателями
