# Практика: Геометрия-2

## 1. Описание предметной области и сущностей
IVisitor<TResult> - интерфейс посетителя, который определяет методы Visit для каждого типа геометрического тела.

Body - абстрактный базовый класс который задает позицию геометрического тела в пространстве и содержит метод Accept для работы с посетителями.

Ball — шар наследник класса Body, у которого есть определенный радиус.

RectangularCuboid - прямоугольный параллелепипед наследник Body, который имеет размеры SizeX, SizeY, SizeZ.

Cylinder - цилиндр наследник Body, имеющий радиус основания и высоту.

CompoundBody - составное тело наследник Body, который содержит в себе список других геометрических тел.

BoundingBoxVisitor - посетитель, который вычисляет минимальный ограничивающий параллелепипед для любых фигур.

BoxifyVisitor - посетитель, который заменяет каждую простую фигуру на её ограничивающий параллелепипед при этом полностью сохраняя структуру составных тел.

## 2. Диаграмма классов (Mermaid)
```mermaid
classDiagram
    direction LR
    class IVisitor~TResult~{
        <<interface>>
        +Visit(Ball body) TResult
        +Visit(RectangularCuboid body) TResult
        +Visit(Cylinder body) TResult
        +Visit(CompoundBody body) TResult
    }
    
    class Body{
        <<abstract>>
        +Vector3 Position
        +Accept~TResult~(IVisitor~TResult~ visitor) TResult*
    }

    class Ball{
        +double Radius
        +Accept~TResult~(IVisitor~TResult~ visitor) TResult
    }

    class RectangularCuboid{
        +double SizeX
        +double SizeY
        +double SizeZ
        +Accept~TResult~(IVisitor~TResult~ visitor) TResult
    }

    class Cylinder{
        +double SizeZ
        +double Radius
        +Accept~TResult~(IVisitor~TResult~ visitor) TResult
    }

    class CompoundBody{
        +IReadOnlyList~body~ Parts
        +Accept~TResult~(IVisitor~TResult~ visitor) TResult
    }

     class BoundingBoxVisitor{
        +RectangularCuboid Result
        +Visit(Ball body) RectangularCuboid
        +Visit(RectangularCuboid body) RectangularCuboid
        +Visit(Cylinder body) RectangularCuboid
        +Visit(CompoundBody body) RectangularCuboid 
    }

    class BoxifyVisitor{
        +Body Result
        +Visit(Ball body) Body
        +Visit(RectangularCuboid body) Body
        +Visit(Cylinder body) Body
        +Visit(CompoundBody body) Body
    }

    IVisitor~RectangularCuboid~ <|.. BoundingBoxVisitor: реализует  
    IVisitor~Body~ <|.. BoxifyVisitor : реализует
    Body<|-- Ball : наследует
    Body<|-- RectangularCuboid : наследует
    Body<|-- Cylinder : наследует
    Body<|-- CompoundBody : наследует
    CompoundBody o-- Body : содержит
    BoundingBoxVisitor ..> RectangularCuboid : возвращает
    BoxifyVisitor ..> BoundingBoxVisitor : использует
```

