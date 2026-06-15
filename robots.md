# Практика: Роботы

## 1. Описание предметной области и сущностей
* Реализуется система управления роботами. У робота есть две основные части:
AI (искусственный интеллект) - мозг робота, который генерирует команды.
Device (устройство) -исполнитель, который выполняет эти команды. 

IRobotAI<out TCommand> - интерфейс ИИ, генерирующий команды.
IDevice<in TCommand> - интерфейс устройства, выполняющий команды.
IMoveCommand и IShooterMoveCommand - интерфейсы команд.
ShooterAI, BuilderAI - конкретные ИИ.
Mover, ShooterMover - конкретные устройства.
Robot<TCommand> - сам робот, объединяющий AI и Device.
Robot - статическая фабрика для удобного создания роботов.
Point - хранит координаты X и Y на карте.*

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram

    class IRobotAI~out TCommand~ {
        <<interface>>
        +GetCommand() TCommand
    }

    class IDevice~in TCommand~ {
        <<interface>>
        +ExecuteCommand(TCommand cmd) string
    }

    class ShooterAI {
        -int tickCounter
        +GetCommand() ShooterCommand
    }

    class BuilderAI {
        -int stepCount
        +GetCommand() BuilderCommand
    }

    class Mover {
        +ExecuteCommand(IMoveCommand moveCmd) string
    }

    class ShooterMover {
        +ExecuteCommand(IShooterMoveCommand shootCmd) string
    }

    class Robot~TCommand~ {
        -IRobotAI~TCommand~ brain
        -IDevice~TCommand~ driver
        +Robot(IRobotAI~TCommand~ brain, IDevice~TCommand~ driver)
        +Start(int steps) IEnumerable~string~
    }

    class RobotFactory {
        <<static>>
        +Create~TCommand~(IRobotAI~TCommand~ brain, IDevice~TCommand~ driver)$ Robot~TCommand~
    }

    class IMoveCommand {
        <<interface>>
        +Point Destination
    }

    class IShooterMoveCommand {
        <<interface>>
        +bool ShouldHide
    }

    class ShooterCommand {
        +Point Destination
        +bool ShouldHide
        +ForCounter(int counter)$ ShooterCommand
    }

    class BuilderCommand {
        +Point Destination
        +bool Build
        +ForCounter(int counter)$ BuilderCommand
    }

    class Point {
        +double X
        +double Y
    }

    RobotFactory ..> Robot~TCommand~ : фабрикует
    Robot~TCommand~ o-- IRobotAI~TCommand~ : brain
    Robot~TCommand~ o-- IDevice~TCommand~ : driver
    ShooterAI ..|> IRobotAI~ShooterCommand~ : реализует AI
    BuilderAI ..|> IRobotAI~BuilderCommand~ : реализует AI
    Mover ..|> IDevice~IMoveCommand~ : реализует устройство
    ShooterMover ..|> IDevice~IShooterMoveCommand~ : реализует устройство
    ShooterCommand ..|> IShooterMoveCommand : реализует
    BuilderCommand ..|> IMoveCommand : реализует
    IShooterMoveCommand --|> IMoveCommand : наследует контракт
    ShooterAI ..> ShooterCommand : создает
    BuilderAI ..> BuilderCommand : создает
    ShooterCommand --> Point : позиция
    BuilderCommand --> Point : позиция
```
