**Также известен как**: Издатель-Подписчик, Слушатель, Observer

##  Суть паттерна

**Наблюдатель** — это поведенческий паттерн проектирования, который создаёт механизм подписки, позволяющий одним объектам следить и реагировать на события, происходящие в других объектах.

##  Проблема

Представьте, что вы имеете два объекта: `Покупатель` и `Магазин`. В магазин вот-вот должны завезти новый товар, который интересен покупателю.

Покупатель может каждый день ходить в магазин, чтобы проверить наличие товара. Но при этом он будет злиться, без толку тратя своё драгоценное время.

С другой стороны, магазин может разослать спам каждому своему покупателю. Многих это расстроит, так как товар специфический, и не всем он нужен.

Получается конфликт: либо покупатель тратит время на периодические проверки, либо магазин тратит ресурсы на бесполезные оповещения.

## Решение

Давайте называть `Издателями` те объекты, которые содержат важное или интересное для других состояние. Остальные объекты, которые хотят отслеживать изменения этого состояния, назовём `Подписчиками`.

Паттерн Наблюдатель предлагает хранить внутри объекта издателя список ссылок на объекты подписчиков, причём издатель не должен вести список подписки самостоятельно. Он предоставит методы, с помощью которых подписчики могли бы добавлять или убирать себя из списка.

[[3df08f680da9168de1817ef0fcdefb91_MD5.jpeg|Open: Pasted image 20250925170757.png]]
![[3df08f680da9168de1817ef0fcdefb91_MD5.jpeg]]
Теперь самое интересное. Когда в издателе будет происходить важное событие, он будет проходиться по списку подписчиков и оповещать их об этом, вызывая определённый метод объектов-подписчиков.

Издателю безразлично, какой класс будет иметь тот или иной подписчик, так как все они должны следовать общему интерфейсу и иметь единый метод оповещения.

[[23eea091f8d13ae08c45e95adb9510d4_MD5.jpeg|Open: Pasted image 20250925170824.png]]
![[23eea091f8d13ae08c45e95adb9510d4_MD5.jpeg]]
Увидев, как складно всё работает, вы можете выделить общий интерфейс, описывающий методы подписки и отписки, и для всех издателей. После этого подписчики смогут работать с разными типами издателей, а также получать оповещения от них через один и тот же метод.
##  Структура

[[e6d4bff1d75571143e70286d8505074d_MD5.jpeg|Open: Pasted image 20250925170917.png]]
![[e6d4bff1d75571143e70286d8505074d_MD5.jpeg]]

1. **Издатель** владеет внутренним состоянием, изменение которого интересно отслеживать подписчикам. Издатель содержит механизм подписки: список подписчиков и методы подписки/отписки.
    
2. Когда внутреннее состояние издателя меняется, он оповещает своих подписчиков. Для этого издатель проходит по списку подписчиков и вызывает их метод оповещения, заданный в общем интерфейсе подписчиков.
    
3. **Подписчик** определяет интерфейс, которым пользуется издатель для отправки оповещения. В большинстве случаев для этого достаточно единственного метода.
    
4. **Конкретные подписчики** выполняют что-то в ответ на оповещение, пришедшее от издателя. Эти классы должны следовать общему интерфейсу подписчиков, чтобы издатель не зависел от конкретных классов подписчиков.
    
5. По приходу оповещения подписчику нужно получить обновлённое состояние издателя. Издатель может передать это состояние через параметры метода оповещения. Более гибкий вариант — передавать через параметры весь объект издателя, чтобы подписчик мог сам получить требуемые данные. Как вариант, подписчик может постоянно хранить ссылку на объект издателя, переданный ему в конструкторе.
    
6. **Клиент** создаёт объекты издателей и подписчиков, а затем регистрирует подписчиков на обновления в издателях.

##  Применимость

 **Когда после изменения состояния одного объекта требуется что-то сделать в других, но вы не знаете наперёд, какие именно объекты должны отреагировать.**

 Описанная проблема может возникнуть при разработке библиотек пользовательского интерфейса, когда вам надо дать возможность сторонним классам реагировать на клики по кнопкам.

Паттерн Наблюдатель позволяет любому объекту с интерфейсом подписчика зарегистрироваться на получение оповещений о событиях, происходящих в объектах-издателях.

 **Когда одни объекты должны наблюдать за другими, но только в определённых случаях.**

 Издатели ведут динамические списки. Все наблюдатели могут подписываться или отписываться от получения оповещений прямо во время выполнения программы.

##  Шаги реализации

1. Разбейте вашу функциональность на две части: независимое ядро и опциональные зависимые части. Независимое ядро станет издателем. Зависимые части станут подписчиками.
    
2. Создайте интерфейс подписчиков. Обычно в нём достаточно определить единственный метод оповещения.
    
3. Создайте интерфейс издателей и опишите в нём операции управления подпиской. Помните, что издатель должен работать только с общим интерфейсом подписчиков.
    
4. Вам нужно решить, куда поместить код ведения подписки, ведь он обычно бывает одинаков для всех типов издателей. Самый очевидный способ — вынести этот код в промежуточный абстрактный класс, от которого будут наследоваться все издатели.
    
    Но если вы интегрируете паттерн в существующие классы, то создать новый базовый класс может быть затруднительно. В этом случае вы можете поместить логику подписки во вспомогательный объект и делегировать ему работу из издателей.
    
5. Создайте классы конкретных издателей. Реализуйте их так, чтобы после каждого изменения состояния они отправляли оповещения всем своим подписчикам.
    
6. Реализуйте метод оповещения в конкретных подписчиках. Не забудьте предусмотреть параметры, через которые издатель мог бы отправлять какие-то данные, связанные с происшедшим событием.
    
    Возможен и другой вариант, когда подписчик, получив оповещение, сам возьмёт из объекта издателя нужные данные. Но в этом случае вы будете вынуждены привязать класс подписчика к конкретному классу издателя.
    
1. Клиент должен создавать необходимое количество объектов подписчиков и подписывать их у издателей.

##  Преимущества и недостатки

| Издатели не зависят от конкретных классов подписчиков и наоборот. | Подписчики оповещаются в случайном порядке. |
| ----------------------------------------------------------------- | ------------------------------------------- |
| Вы можете подписывать и отписывать получателей на лету.           |                                             |
| Реализует _принцип открытости/закрытости_.                        |                                             |


## Конкретно

```cs
using System;
using System.Collections.Generic;

// Общий интерфейс подписчиков
public interface IEventListener
{
    void Update(string filename);
}

// Базовый класс-издатель. Содержит код управления подписчиками
// и их оповещения.
public class EventManager
{
    private Dictionary<string, List<IEventListener>> listeners = new Dictionary<string, List<IEventListener>>();

    public void Subscribe(string eventType, IEventListener listener)
    {
        if (!listeners.ContainsKey(eventType))
        {
            listeners[eventType] = new List<IEventListener>();
        }
        listeners[eventType].Add(listener);
    }

    public void Unsubscribe(string eventType, IEventListener listener)
    {
        if (listeners.ContainsKey(eventType))
        {
            listeners[eventType].Remove(listener);
        }
    }

    public void Notify(string eventType, string data)
    {
        if (listeners.ContainsKey(eventType))
        {
            foreach (var listener in listeners[eventType])
            {
                listener.Update(data);
            }
        }
    }
}

// Вспомогательный класс File для демонстрации
public class File
{
    public string Name { get; private set; }
    public string Path { get; private set; }

    public File(string path)
    {
        Path = path;
        Name = System.IO.Path.GetFileName(path);
    }

    public void Write()
    {
        // Реализация записи файла
        Console.WriteLine($"Запись файла: {Name}");
    }
}

// Конкретный класс-издатель
public class Editor
{
    public EventManager Events { get; private set; }
    private File file;

    public Editor()
    {
        Events = new EventManager();
    }

    // Методы бизнес-логики, которые оповещают подписчиков об изменениях
    public void OpenFile(string path)
    {
        this.file = new File(path);
        Events.Notify("open", file.Name);
    }

    public void SaveFile()
    {
        if (file != null)
        {
            file.Write();
            Events.Notify("save", file.Name);
        }
    }
}

// Конкретный подписчик - логирование
public class LoggingListener : IEventListener
{
    private string logPath;
    private string message;

    public LoggingListener(string logPath, string message)
    {
        this.logPath = logPath;
        this.message = message;
    }

    public void Update(string filename)
    {
        string logMessage = message.Replace("%s", filename);
        Console.WriteLine($"[LOG {logPath}] {logMessage}");
        // Здесь была бы реальная запись в файл
        // System.IO.File.AppendAllText(logPath, logMessage + Environment.NewLine);
    }
}

// Конкретный подписчик - email уведомления
public class EmailAlertsListener : IEventListener
{
    private string email;
    private string message;

    public EmailAlertsListener(string email, string message)
    {
        this.email = email;
        this.message = message;
    }

    public void Update(string filename)
    {
        string emailMessage = message.Replace("%s", filename);
        Console.WriteLine($"[EMAIL to {email}] {emailMessage}");
        // Здесь была бы реальная отправка email
        // system.email(email, emailMessage);
    }
}

// Приложение для демонстрации работы
public class Application
{
    public void Config()
    {
        Editor editor = new Editor();

        var logger = new LoggingListener(
            "/path/to/log.txt",
            "Someone has opened file: %s");
        editor.Events.Subscribe("open", logger);

        var emailAlerts = new EmailAlertsListener(
            "admin@example.com",
            "Someone has changed the file: %s");
        editor.Events.Subscribe("save", emailAlerts);

        // Демонстрация работы
        editor.OpenFile("document.txt");
        editor.SaveFile();
    }
}

// Пример использования
class Program
{
    static void Main(string[] args)
    {
        Application app = new Application();
        app.Config();
        
        // Дополнительный пример
        Editor editor = new Editor();
        
        editor.Events.Subscribe("open", new LoggingListener("log.txt", "Файл открыт: %s"));
        editor.Events.Subscribe("save", new EmailAlertsListener("user@test.com", "Файл сохранен: %s"));
        
        editor.OpenFile("test.docx");
        editor.SaveFile();
    }
}
```


```plantuml_class
@startuml
!theme plain

title Упрощенная диаграмма паттерна Наблюдатель

interface IEventListener {
    + Update(filename: string): void
}

class EventManager {
    + Subscribe(eventType: string, listener: IEventListener): void
    + Unsubscribe(eventType: string, listener: IEventListener): void
    + Notify(eventType: string, data: string): void
}

class Editor {
    + Events: EventManager
    + OpenFile(path: string): void
    + SaveFile(): void
}

class LoggingListener {
    + Update(filename: string): void
}

class EmailAlertsListener {
    + Update(filename: string): void
}

' Связи
IEventListener <|.. LoggingListener
IEventListener <|.. EmailAlertsListener
Editor *-- EventManager
EventManager o-- IEventListener

Editor -> IEventListener : notify
IEventListener <.. Editor : subscribe

@enduml
```


```plantuml_seq
@startuml
title Последовательность работы паттерна Наблюдатель

actor User
participant Application
participant Editor
participant EventManager
participant LoggingListener
participant EmailAlertsListener

User -> Application: Запуск Config()
Application -> Editor: new Editor()
Application -> LoggingListener: new LoggingListener()
Application -> Editor: Subscribe("open", logger)
Editor -> EventManager: Subscribe("open", logger)

Application -> EmailAlertsListener: new EmailAlertsListener()
Application -> Editor: Subscribe("save", emailAlerts)
Editor -> EventManager: Subscribe("save", emailAlerts)

User -> Editor: OpenFile("document.txt")
Editor -> EventManager: Notify("open", "document.txt")
EventManager -> LoggingListener: Update("document.txt")
LoggingListener -> LoggingListener: Запись в лог

User -> Editor: SaveFile()
Editor -> EventManager: Notify("save", "document.txt")
EventManager -> EmailAlertsListener: Update("document.txt")
EmailAlertsListener -> EmailAlertsListener: Отправка email

@enduml
```