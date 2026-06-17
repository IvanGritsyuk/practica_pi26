# Практика: Роботы

## 1. Описание предметной области и сущностей
* Реализуется система управления роботами. У робота есть две основные части:
AI (искусственный интеллект) - мозг робота, который генерирует команды.
Device (устройство) -исполнитель, который выполняет эти команды. 
ICommand - базовый интерфейс для любых действий.
IMoveCommand - интерфейс команды перемещения, содержит целевую точку.
ICombatMoveCommand - интерфейс боевой команды, расширяет перемещение флагом уклонения.
IRobotAI<out T> - интерфейс ИИ с ковариантным параметром, генерирует команды.
IDevice<in T> - интерфейс устройства с контрвариантным параметром, выполняет команды.
CombatAI, EngineeringAI - конкретные ИИ для боевых и строительных задач.
LocomotiveActuator, CombatActuator - конкретные устройства для перемещения и боя.
AutonomousRobot<TCommand> - основной класс робота, объединяющий AI и Device.
AssaultOrder, ConstructionOrder - классы данных с параметрами конкретных команд.
Coordinates - хранит координаты X и Y на карте.

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    direction LR

    class ICommand {
        <<interface>>
    }

    class IMoveCommand {
        <<interface>>
        +Coordinates TargetPoint
    }

    class ICombatMoveCommand {
        <<interface>>
        +bool EvadeMode
    }

    class IRobotAI~out T~ {
        <<interface>>
        +ComputeNextCommand() T
    }

    class IDevice~in T~ {
        <<interface>>
        +ProcessCommand(T cmd) string
    }

    class CombatAI {
        -int _cycleCount
        +ComputeNextCommand() AssaultOrder
    }

    class EngineeringAI {
        -int _operationStep
        +ComputeNextCommand() ConstructionOrder
    }

    class LocomotiveActuator {
        +ProcessCommand(IMoveCommand movement) string
    }

    class CombatActuator {
        +ProcessCommand(ICombatMoveCommand combatCmd) string
    }

    class AutonomousRobot~TCommand~ {
        -IRobotAI~TCommand~ _controller
        -IDevice~TCommand~ _actuator
        +AutonomousRobot(IRobotAI~TCommand~ ai, IDevice~TCommand~ device)
        +ExecuteCycle(int iterations) IEnumerable~string~
    }

    class AssaultOrder {
        +Coordinates TargetPoint
        +bool EvadeMode
        +BuildSequence(int id)$ AssaultOrder
    }

    class ConstructionOrder {
        +Coordinates TargetPoint
        +bool DeployStructure
        +BuildSequence(int id)$ ConstructionOrder
    }

    class Coordinates {
        +double PositionX
        +double PositionY
    }

    ICommand <|-- IMoveCommand : определяет движение
    IMoveCommand <|-- ICombatMoveCommand : уточняет поведение
    ICombatMoveCommand <|.. AssaultOrder : реализует боевую команду
    IMoveCommand <|.. ConstructionOrder : реализует строительную команду
    IRobotAI~AssaultOrder~ <|.. CombatAI : обеспечивает тактику
    IRobotAI~ConstructionOrder~ <|.. EngineeringAI : управляет строительством 
    IDevice~IMoveCommand~ <|.. LocomotiveActuator : приводит в движение
    IDevice~ICombatMoveCommand~ <|.. CombatActuator : управляет огнём
    AutonomousRobot~TCommand~ *-- IRobotAI~TCommand~ : полагается на ИИ
    AutonomousRobot~TCommand~ *-- IDevice~TCommand~ : передаёт исполнение
    AssaultOrder --> Coordinates : координаты
    ConstructionOrder --> Coordinates : координаты
```
