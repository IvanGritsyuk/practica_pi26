# Практика: Генератор отчетов

## 1. Описание предметной области и сущностей
В системе моделируется гибкий генератор статистических отчетов о погодных измерениях. Она принимает набор измерений температуры и влажности за несколько дней, вычисляет выбранные статистические показатели и оформляет результат в одном из двух форматов HTML или Markdown.

Measurement - хранит данные одного измерения: температуру воздуха и влажность. 
MeanAndStd - вспомогательный класс который содержит результат вычислений средней температуры и стандартного отклонения.
IMetricsProvider - интерфейс который задает правила для всех классов, отвечающих за вычисление статистики.
HtmlReportRenderer - оформляет отчет в формате HTML.
MarkdownReportRenderer - оформляет отчет в формате Markdown.
MeanAndStdMetrics - принимает список чисел, вычисляет их среднее арифметическое и стандартное отклонение.
MedianMetrics - принимает список чисел, сортирует их и находит медиану.
ReportMaker - основной и главный класс который собирает готовый отчет. В конструктор получает оформитель, вычислитель и заголовок будущего отчета. 
ReportMakerHelper - спомогательный класс который предоставляет готовые методы для получения всех четырех типов отчетов.

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    direction TB

    class Measurement {
        +double Temperature
        +double Humidity
    }

    class MeanAndStd {
        +double Mean
        +double Std
        +ToString() string
    }

    class IReportRenderer {
        <<interface>>
        +RenderHeader(string) string
        +OpenList() string
        +RenderRow(string, string) string
        +CloseList() string
    }

    class IMetricsProvider {
        <<interface>>
        +Execute(IEnumerable~double~) object
    }

    class HtmlReportRenderer {
        +RenderHeader(string) string
        +OpenList() string
        +RenderRow(string, string) string
        +CloseList() string
    }

    class MarkdownReportRenderer {
        +RenderHeader(string) string
        +OpenList() string
        +RenderRow(string, string) string
        +CloseList() string
    }

    class MeanAndStdMetrics {
        +Execute(IEnumerable~double~) object
    }

    class MedianMetrics {
        +Execute(IEnumerable~double~) object
    }

    class ReportMaker {
        -IReportRenderer _renderer
        -IMetricsProvider _metrics
        -string _title
        +ReportMaker(IReportRenderer, IMetricsProvider, string)
        +CreateReport(IEnumerable~Measurement~) string
    }

    class ReportMakerHelper {
        <<static>>
        +MeanAndStdHtmlReport(IEnumerable~Measurement~) string
        +MedianMarkdownReport(IEnumerable~Measurement~) string
        +MeanAndStdMarkdownReport(IEnumerable~Measurement~) string
        +MedianHtmlReport(IEnumerable~Measurement~) string
    }

    IReportRenderer <|.. HtmlReportRenderer : реализует
    IReportRenderer <|.. MarkdownReportRenderer : реализует
    IMetricsProvider <|.. MeanAndStdMetrics : реализует
    IMetricsProvider <|.. MedianMetrics : реализует

    ReportMaker *-- IReportRenderer : компонует
    ReportMaker *-- IMetricsProvider : компонует
    ReportMaker ..> Measurement : использует данные
    MeanAndStdMetrics ..> MeanAndStd : создает структуру
    
    ReportMakerHelper ..> ReportMaker : создает
    ReportMakerHelper ..> HtmlReportRenderer : создает
    ReportMakerHelper ..> MarkdownReportRenderer : создает
    ReportMakerHelper ..> MeanAndStdMetrics : создает
    ReportMakerHelper ..> MedianMetrics : создает
```
