# Практика: GraphViz

## 1. Описание предметной области и сущностей
*Система реализует Fluent API для построения графов в формате DOT. DotGraphBuilder - статический класс, который создаёт граф через методы DirectedGraph и UndirectedGraph. IGraphBuilder, INodeBuilder и IEdgeBuilder управляют добавлением элементов, а INodeAttributes и IEdgeAttributes настраивают их свойства.Внутренние классы GraphBuilder, NodeBuilder и EdgeBuilder реализуют эти интерфейсы. Конфигурация выполняется через метод With, который принимает лямбду с настройками и возвращает IGraphBuilder для продолжения цепочки. Graph, GraphNode и GraphEdge хранят данные, NodeShape - формы вершин.*

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    class DotGraphBuilder {
        +DirectedGraph(string name)$ IGraphBuilder
        +UndirectedGraph(string name)$ IGraphBuilder
    }

    class IGraphBuilder {
        <<interface>>
        +AddNode(string id) INodeBuilder
        +AddEdge(string from, string to) IEdgeBuilder
        +Build() string
    }

    class INodeBuilder {
        <<interface>>
        +With(Action~INodeAttributes~ action) IGraphBuilder
        +AddNode(string id) INodeBuilder
        +AddEdge(string from, string to) IEdgeBuilder
        +Build() string
    }

    class IEdgeBuilder {
        <<interface>>
        +With(Action~IEdgeAttributes~ action) IGraphBuilder
        +AddNode(string id) INodeBuilder
        +AddEdge(string from, string to) IEdgeBuilder
        +Build() string
    }

    class INodeAttributes {
        <<interface>>
        +Color(string color) INodeAttributes
        +Shape(NodeShape shape) INodeAttributes
        +FontSize(int size) INodeAttributes
        +Label(string label) INodeAttributes
    }

    class IEdgeAttributes {
        <<interface>>
        +Color(string color) IEdgeAttributes
        +FontSize(int size) IEdgeAttributes
        +Label(string label) IEdgeAttributes
        +Weight(double weight) IEdgeAttributes
    }

    class GraphBuilder {
        -Graph _graph
        +AddNode(string) NodeBuilder
        +AddEdge(string, string) EdgeBuilder
        +Build() string
    }

    class NodeBuilder {
        -GraphBuilder _ctx
        -GraphNode _node
        +With(Action~INodeAttributes~) GraphBuilder
        +AddNode(string) NodeBuilder
        +AddEdge(string, string) EdgeBuilder
        +Build() string
    }

    class EdgeBuilder {
        -GraphBuilder _ctx
        -GraphEdge _edge
        +With(Action~IEdgeAttributes~) GraphBuilder
        +AddNode(string) NodeBuilder
        +AddEdge(string, string) EdgeBuilder
        +Build() string
    }

    class NodeConfig {
        -GraphNode _node
        +Color(string) INodeAttributes
        +Shape(NodeShape) INodeAttributes
        +FontSize(int) INodeAttributes
        +Label(string) INodeAttributes
    }

    class EdgeConfig {
        -GraphEdge _edge
        +Color(string) IEdgeAttributes
        +Weight(double) IEdgeAttributes
        +FontSize(int) IEdgeAttributes
        +Label(string) IEdgeAttributes
    }

    class Graph {
        <<external>>
        +AddNode(string) GraphNode
        +AddEdge(string, string) GraphEdge
    }

    class GraphNode {
        <<external>>
        +Dictionary Attributes
    }

    class GraphEdge {
        <<external>>
        +Dictionary Attributes
    }

    class NodeShape {
        <<enumeration>>
        Box
        Ellipse
    }

    IGraphBuilder <|.. GraphBuilder : реализует
    INodeBuilder <|.. NodeBuilder : реализует
    IEdgeBuilder <|.. EdgeBuilder : реализует
    INodeAttributes <|.. NodeConfig : реализует
    IEdgeAttributes <|.. EdgeConfig : реализует
    DotGraphBuilder ..> GraphBuilder : создает билдер
    GraphBuilder o-- Graph : наполняет граф
    NodeBuilder o-- GraphBuilder : хранит контекст
    EdgeBuilder o-- GraphBuilder : хранит контекст
    NodeConfig o-- GraphNode : настраивает узел
    EdgeConfig o-- GraphEdge : настраивает ребро
    GraphBuilder ..> NodeBuilder : создает NodeBuilder
    GraphBuilder ..> EdgeBuilder : создает EdgeBuilder
    NodeBuilder ..> NodeConfig : вызывает лямбду
    EdgeBuilder ..> EdgeConfig : вызывает лямбду
```
