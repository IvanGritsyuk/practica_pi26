# Практика: Сбои

## 1. Описание предметной области и сущностей
В системе ведется учет об устройствах на которых произошел сбой до определенной даты. Device хранит информацию об устройстве, FailureType хранит тип сбоя, Failure хранит в себе информацию о сбое в конкретном устройстве, ReportMaker является основным классом который принимает список устройств и список сбоев, затем определяет на каких устройсвах были критические сбои до определенной даты и затем фиксирует данный устройства.

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram

    class Device {
        +int Id
        +string Name
        +Device(int id, string name)
    }
    
    class FailureType{
        <<enumeration>>
        UnexpectedShutdown = 0,
        ShortNonResponding = 1,
        HardwareFailures = 2,
        ConnectionProblems = 3, 
    }

    class Failure {
        +int DeviceId
        +int FailureType
        +DateTime Date 
        +Failure(int deviceId, int failureType, DateTime date)
        +IsFailureSerious()
    }

    class ReportMaker{
        
    +List~string~ FindDevicesFailedBeforeDateObsolete(int day, int month, int year, int[] failureTypes, int[] deviceId, object[][] times, List<Dictionary<string, object>> devices)

    +List~string~ FindDevicesFailedBeforeDate(List~Device~	allDevices, List~Failure~ failures, DateTime date)
    }

    ReportMaker ..> Device : принимает список устройств 
    ReportMaker ..> Failure : принимает тип сбоя 
    Failure --> FailureType : содержит тип сбоя
```
