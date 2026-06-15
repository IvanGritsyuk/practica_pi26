# Практика: Сбои

## 1. Описание предметной области и сущностей
В системе ведется учет об устройствах на которых произошел сбой до определенной даты. Device хранит информацию об устройстве, FailureType хранит тип сбоя, Failure хранит в себе информацию о сбое в конкретном устройстве, ReportMaker является основным классом который принимает список устройств и список сбоев, затем определяет на каких устройсвах были критические сбои до определенной даты и затем фиксирует данный устройства.

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    direction LR
    class Device {
        +int Id
        +string Name
        +Device(int id, string name)
    }
    
    class FailureType {
        <<enumeration>>
        UnexpectedShutdown = 0
        ShortNonResponding = 1
        HardwareFailures = 2
        ConnectionProblems = 3
    }

    class Failure {
        +int TargetDeviceId
        +FailureType CurrentType
        +DateTime CrashDate 
        +Failure(int targetDeviceId, FailureType currentType, DateTime crashDate)
        +IsCritical() bool
    }

    class ReportMaker {
        +FindDevicesFailedBeforeDateObsolete(int day, int month, int year, int[] codes, int[] ids, object[][] timestamps, List~Dictionary~ items) List~string~
        +FindDevicesFailedBeforeDate(DateTime deadline, List~Failure~ failureEvents, List~Device~ deviceList) List~string~
    }

    ReportMaker ..> Device : анализирует список устройств
    ReportMaker ..> Failure : анализирует список сбоев
    Failure --> FailureType : содержит тип сбоя
```
