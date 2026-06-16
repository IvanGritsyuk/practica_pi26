# Практика: Сбои

## 1. Описание предметной области и сущностей
В системе ведётся учёт устройств, на которых произошли сбои до определённой даты. 
Device хранит информацию об устройстве. 
FailureType содержит возможные типы сбоев. 
Failure представляет информацию о конкретном сбое устройства, тип и дату возникновения.
FailureLog отвечает за хранение и предоставление сведений о зарегистрированных сбоях. Класс FailureSeverity определяет, является ли определёный тип сбоя критическим.
ReportMaker он получает информацию об устройствах и зарегистрированных сбоях, использует FailureLog для доступа к данным о сбоях и FailureSeverity для определения их критичности. После этого формируется список устройств, на которых произошли критические сбои до заданной даты


## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    direction LR

    class Device {
        +int Id
        +string Name
    }

    class FailureType {
        <<enumeration>>
        UnexpectedShutdown
        ShortNonResponding
        HardwareFailures
        ConnectionProblems
    }

    class Failure {
        +int DeviceId
        +FailureType Type
        +DateTime OccurredAt
    }

    class FailureLog {
        +List~Failure~ Records
        +GetFailuresBefore(DateTime date) List~Failure~
    }

    class FailureSeverity {
        +IsSerious(FailureType type) bool
    }

    class ReportMaker {
        +FindDevicesFailedBeforeDateObsolete(int day, int month, int year, int[] codes, int[] ids, object[][] timestamps, List~Dictionary~ items) List~string~
        +FindDevicesFailedBeforeDate(DateTime date, List~Device~ devices, FailureLog log) List~string~
    }

    Failure --> FailureType : имеет тип
    FailureLog *-- Failure : хранит сбои 
    ReportMaker ..> Device : формирует отчет об устройствах
    ReportMaker ..> FailureLog : получает данные о сбоях
    ReportMaker ..> FailureSeverity : определяет серьезность сбоя
    FailureSeverity ..> FailureType : анализирует тип сбоя
```
