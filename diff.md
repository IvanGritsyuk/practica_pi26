# Практика: Дифференцирование

## 1. Описание предметной области и сущностей
Программа вычисляет производные, представляя математические функции в виде деревьев из  класса Expression например, константы, переменные, бинарные операции. Главный класс Algebra отвечает за сам процесс дифференцирования: он проходит по этому дереву и применяет математические правила, создавая новое дерево-производную. Разделение на данные узлы дерева и логику класс Algebra делает код понятным и позволяет легко добавлять новые функции в будущем.

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    direction LR

    class Algebra {
        <<static>>
        +Differentiate(Expression~Func~double,double~~) Expression~Func~double,double~~
        -ApplyRule(Expression node, ParameterExpression variable) Expression
    }

    class Expression {
        <<abstract>>
        +NodeType ExpressionType
    }

    class ConstantExpression {
        +Value object
    }

    class ParameterExpression {
        +Name string
    }

    class BinaryExpression {
        +Left Expression
        +Right Expression
    }

    class UnaryExpression {
        +Operand Expression
    }

    class MethodCallExpression {
        +Method MethodInfo
        +Arguments Expression[]
    }

    class ExpressionType {
        <<enumeration>>
        Constant
        Parameter
        Add
        Multiply
        Negate
        Call
    }

    Expression <|-- ConstantExpression : расширяет
    Expression <|-- ParameterExpression : расширяет
    Expression <|-- BinaryExpression : расширяет
    Expression <|-- UnaryExpression : расширяет
    Expression <|-- MethodCallExpression : расширяет

    BinaryExpression --> ExpressionType : определяет тип операции
    UnaryExpression --> ExpressionType : определяет тип унарной операции
    MethodCallExpression --> ExpressionType : определяет тип вызова метода

    Algebra ..> Expression : анализирует и преобразует дерево
    Algebra ..> ParameterExpression : использует 
    Algebra ..> ExpressionType : проверяет тип узла для применения правил
```