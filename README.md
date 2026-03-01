# GDP Analysis System - Phase 2: Modular Orchestration

This project transitions a script-based GDP data analysis tool into a robust **Modular Architecture** by strictly applying the **Dependency Inversion Principle (DIP)**. The core business logic is entirely decoupled from data ingestion and output presentation, communicating strictly through Python `Protocols`.

## 🏗️ System Architecture

The application is refactored into four distinct logical packages. The Core module acts as the absolute authority, defining the structural "shape" of the data it accepts and the methods it expects from external sinks.

* **Main Module (`main.py`)**: The Orchestrator. It reads `config.json` to determine runtime behavior, utilizes a dictionary-based factory to instantiate the required plugins, and performs Dependency Injection to wire the components together.
* **Core Module (`core/`)**: The Domain. Owns the business logic and transformation rules using purely functional programming constructs (`map`, `filter`, `lambda`). It defines the `PipelineService` (Inbound) and `DataSink` (Outbound) protocols.
* **Input Module (`plugins/inputs.py`)**: The Source. Provides `CSVReader` and `JSONReader`. These classes remain completely blind to the Core's internal logic, interacting solely by calling the injected `PipelineService`.
* **Output Module (`plugins/outputs.py`)**: The Sink. Provides `ConsoleWriter` and a Plotly-powered `GraphicsChartWriter`. These strictly satisfy the `DataSink` protocol defined by the Core to render the data dynamically.

## ✨ Features & Analysis Capabilities

The `TransformationEngine` calculates the following metrics dynamically based on the provided configuration parameters:
1.  **Top 10 Countries by GDP** for a specific continent and year.
2.  **Bottom 10 Countries by GDP** for a specific continent and year.
3.  **GDP Growth Rates** for countries over a defined time span.
4.  **Average GDP by Continent** visualized over time.
5.  **Total Global GDP Trend** visualized over time.
6.  **Fastest Growing Continent** within the data range.
7.  **Consistent GDP Decline** (identifying countries with consecutive years of decline).
8.  **Continental Contributions** to the global GDP (Percentage breakdown).

## 📂 Project Structure

```text
project_root/
│
├── main.py                # The Orchestrator / Entry Point
├── config.json            # Swappable configuration for pipelines and parameters
├── README.md              # Project documentation
│
├── core/
│   ├── __init__.py
│   ├── contracts.py       # Defines DataSink & PipelineService Protocols
│   └── engine.py          # TransformationEngine (Functional Math Logic)
│
├── plugins/
│   ├── __init__.py
│   ├── inputs.py          # CSVReader, JSONReader
│   └── outputs.py         # ConsoleWriter, GraphicsChartWriter (Plotly)
│
└── data/
    └── gdp_data.csv       # Raw source data
