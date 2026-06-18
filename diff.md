# Практика: Дифференцирование

## 1. Описание предметной области и сущностей
Программа вычисляет производные функций одной переменной, которые представлены в виде дерева выражений. Главный класс Algebra запускает процесс дифференцирования и передаёт работу классу DifferentiationEngine. Этот класс просматривает узлы дерева и выбирает нужное правило для вычисления производной : ConstantRule, ParameterRule, BinaryRule, TrigonometricRule. Такой подход позволяет легко расширять систему поддержкой новых математических операций и функций без изменения уже существующих компонентов.

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    direction LR

    class Algebra {
        <<static>>
        +Differentiate(Expression~Func~double,double~~) Expression~Func~double,double~~
    }

    class DifferentiationEngine {
        -rules Dictionary~ExpressionType, IDifferentiationRule~
        +Differentiate(Expression, ParameterExpression) Expression
    }

    class IDifferentiationRule {
        <<interface>>
        +Differentiate(Expression, ParameterExpression) Expression
    }

    class ConstantRule {
        +Differentiate(Expression, ParameterExpression) Expression
    }

    class ParameterRule {
        +Differentiate(Expression, ParameterExpression) Expression
    }

    class BinaryRule {
        +Differentiate(Expression, ParameterExpression) Expression
    }

    class TrigonometricRule {
        +Differentiate(Expression, ParameterExpression) Expression
    }

    class Expression {
        <<abstract>>
        +NodeType ExpressionType
    }

    class ParameterExpression {
        +Name string
    }

    class ExpressionType {
        <<enumeration>>
        Constant
        Parameter
        Add
        Multiply
        Call
    }

    Algebra ..> DifferentiationEngine : использует 

    DifferentiationEngine o--> IDifferentiationRule : хранит

    IDifferentiationRule <|.. ConstantRule : реализует
    IDifferentiationRule <|.. ParameterRule : реализует
    IDifferentiationRule <|.. BinaryRule : реализует
    IDifferentiationRule <|.. TrigonometricRule : реализует

    DifferentiationEngine ..> Expression : анализирует дерево выражения
    DifferentiationEngine ..> ParameterExpression : использует переменную дифференцирования

    ConstantRule ..> ExpressionType : проверяет
    ParameterRule ..> ExpressionType : проверяет
    BinaryRule ..> ExpressionType : обрабатывает
    TrigonometricRule ..> ExpressionType : обрабатывает

    Expression --> ExpressionType : определяет тип узла
```