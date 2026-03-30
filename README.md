# finpulse-precious-metals-pipeline-DataManagment
End-to-end data pipeline for precious metals trading intelligence — Gold &amp; Silver market analysis with sentiment enrichment, polyglot persistence (MongoDB + Neo4j), and technical indicators.
<p align="center">
  <h1 align="center">⚜️ FinPulse</h1>
  <p align="center">
    <strong>Precious Metals Trading Intelligence Pipeline</strong>
  </p>
  <p align="center">
    End-to-end data management pipeline integrating financial price data and news sentiment<br>
    for gold (XAU/USD) and silver (XAG/USD) market analysis.
  </p>
  <p align="center">
    <img src="https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white" alt="Python">
    <img src="https://img.shields.io/badge/MongoDB-7.0-47A248?logo=mongodb&logoColor=white" alt="MongoDB">
    <img src="https://img.shields.io/badge/Neo4j-5-4581C3?logo=neo4j&logoColor=white" alt="Neo4j">
    <img src="https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white" alt="Docker">
    <img src="https://img.shields.io/badge/Jupyter-Lab-F37626?logo=jupyter&logoColor=white" alt="Jupyter">
    <img src="https://img.shields.io/badge/Kafka-Available-231F20?logo=apachekafka&logoColor=white" alt="Kafka">
  </p>
</p>

---

## 📋 Overview

**FinPulse** is a complete data management project built for the *Data Management* course at Università degli Studi di Milano-Bicocca (A.Y. 2025/2026). It constructs a full data science pipeline — from acquisition to analysis — covering:

- **Data Acquisition** from two heterogeneous sources (financial API + web scraping)
- **Data Profiling & Quality Assessment** with completeness, accuracy, and consistency metrics
- **Data Integration & Enrichment** combining price data with NLP sentiment scores and 13 technical indicators
- **Polyglot Persistent Storage** using MongoDB (document store) and Neo4j (graph database)
- **Analytical Querying** with 7 MongoDB aggregation pipelines and 7 Neo4j Cypher traversal queries
- **Research Analysis** answering 3 research questions with statistical tests

The pipeline tracks 4 assets over a 2-year window (March 2024 – March 2026): **Gold**, **Silver**, **U.S. Dollar Index (DXY)**, and **S&P 500**.

---

## 🔬 Research Questions

| # | Question | Key Finding |
|---|----------|-------------|
| **RQ1** | Does news sentiment correlate with gold price movements within 24 hours? | Weak negative correlation (r ≈ -0.19), not statistically significant with N=77. Contrarian pattern observed. |
| **RQ2** | Which data quality dimensions are most critical for near-real-time financial data? | Sentiment coverage (15.3%) is the most critical dimension — a structural limitation of free RSS acquisition. |
| **RQ3** | Does integrating sentiment + technical indicators produce better trading signals? | Buy-and-hold (+98.38%) outperformed all active strategies during gold's strong uptrend. Integration value is conditional on market regime. |

---

## 🏗️ Architecture

The project adopts a **polyglot persistence** architecture orchestrated with Docker Compose:

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│    NB01      │    │    NB02      │    │    NB03      │    │    NB04      │    │    NB05      │
│ Acquisition  │───▶│  Profiling   │───▶│ Integration  │───▶│   Queries    │───▶│  Analysis    │
│              │    │              │    │              │    │              │    │              │
│ yfinance API │    │ Completeness │    │ VADER+TB     │    │ 7 MongoDB    │    │ RQ1,RQ2,RQ3  │
│ RSS Scraping │    │ Accuracy     │    │ 13 Tech Ind. │    │ 7 Neo4j      │    │ Stat. Tests  │
└─────────────┘    │ Consistency  │    │ Neo4j Graph  │    └─────────────┘    └─────────────┘
                   └─────────────┘    └─────────────┘
                          │                  │
                          ▼                  ▼
                   ┌─────────────┐    ┌─────────────┐
                   │  MongoDB    │    │   Neo4j     │
                   │  7.0        │    │  5 Comm.    │
                   │  8 collections│   │  585 nodes  │
                   │  2,014 docs │    │  1,090 rels │
                   └─────────────┘    └─────────────┘
```

<!-- Uncomment and replace with your screenshot:
![Architecture Diagram](images/architecture.png)
-->

---

## 📊 Key Results

<table>
  <tr>
    <td align="center"><strong>2,014</strong><br>Price Records</td>
    <td align="center"><strong>156</strong><br>News Headlines</td>
    <td align="center"><strong>585</strong><br>Graph Nodes</td>
    <td align="center"><strong>1,090</strong><br>Graph Relationships</td>
  </tr>
  <tr>
    <td align="center"><strong>+98.38%</strong><br>Gold Return (2Y)</td>
    <td align="center"><strong>+0.92</strong><br>Gold-Silver Correlation</td>
    <td align="center"><strong>99.97%</strong><br>Post-Integration Quality</td>
    <td align="center"><strong>47</strong><br>News Publishers</td>
  </tr>
</table>

---

## 🗄️ Data Sources

| Source | Type | Method | Records |
|--------|------|--------|---------|
| **Yahoo Finance** | OHLCV price data (4 assets) | `yfinance` Python API | 2,014 |
| **Google News RSS** | Financial news headlines | `BeautifulSoup` web scraping | ~100/run |
| **Investing.com RSS** | Commodity news headlines | `BeautifulSoup` web scraping | ~10/run |

No external API keys required — all data sources are free and open.

---

## 🛠️ Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Orchestration** | Docker Compose | Service management & reproducibility |
| **Development** | JupyterLab | Interactive notebook environment |
| **Document Store** | MongoDB 7.0 | Raw data, integrated data, metadata collections |
| **Graph Database** | Neo4j 5 Community | Asset correlation network, temporal chains |
| **Message Broker** | Apache Kafka + Zookeeper | Available for streaming extensions |
| **Language** | Python 3.11 | Pipeline orchestration, analysis |
| **NLP** | VADER + TextBlob | Dual-model sentiment analysis |
| **Data** | yfinance, BeautifulSoup | Financial API + web scraping |
| **Analysis** | pandas, scipy, matplotlib | Statistical testing & visualization |

---

## 🚀 Getting Started

### Prerequisites

- Docker Desktop (Windows/macOS) or Docker Engine (Linux)
- Docker Compose v2
- Minimum 8 GB RAM allocated to Docker
- Internet connection for initial data acquisition

### Setup

```bash

# 1. Start all services
docker compose up -d

# 2. Access JupyterLab
# Open http://localhost:8888 in your browser

# 3. Execute notebooks in strict sequential order
# NB01 → NB02 → NB03 → NB04 → NB05
```

### Service Endpoints

| Service | Endpoint |
|---------|----------|
| JupyterLab | http://localhost:8888 |
| MongoDB | `mongodb://mongo:27017` (from notebooks) |
| Neo4j Browser | http://localhost:7474 (`neo4j` / `password`) |
| Neo4j Bolt | `bolt://neo4j:7687` (from notebooks) |

> **Important:** All database connections inside notebooks use Docker service names (`mongo`, `neo4j`) — not `localhost`.

---

## 📁 Project Structure

```
FinPulse/
├── README.md                          # This file
├── LICENSE
├── .gitignore
│
├── notebooks/
│   ├── NB01_Data_Acquisition.ipynb    # Price download + news scraping
│   ├── NB02_Data_Profiling.ipynb      # Statistical profiling + quality assessment
│   ├── NB03_Data_Integration.ipynb    # Sentiment + technical indicators + Neo4j
│   ├── NB04_Storage_Queries.ipynb     # MongoDB aggregations + Cypher traversals
│   └── NB05_Analysis.ipynb            # Research questions + statistical tests
│
├── docs/
│   ├── FinPulse_Report_EN.pdf         # Full project report (English)
│   ├── FinPulse_Report_IT.pdf         # Full project report (Italian)
│   ├── FinPulse_Presentation_EN.pptx  # 18-slide presentation (English)
│   └── FinPulse_Presentation_IT.pptx  # 18-slide presentation (Italian)
```

---

## 📓 Notebooks

| Notebook | Phase | Description | Key Output |
|----------|-------|-------------|------------|
| **NB01** | Acquisition | Downloads 2 years of OHLCV data via yfinance API; scrapes news headlines from Google News and Investing.com RSS feeds using BeautifulSoup | `prices` (2,014 docs), `news` (156 docs) collections |
| **NB02** | Profiling | Computes statistical profiles, assesses completeness/accuracy/consistency, detects outliers via IQR method | `quality_reports`, `quality_issues` collections |
| **NB03** | Integration | Runs VADER+TextBlob sentiment analysis, computes 13 technical indicators, performs temporal join, builds Neo4j graph model | `integrated_data` collection, Neo4j graph (585 nodes) |
| **NB04** | Queries | Executes 7 MongoDB aggregation pipelines (MQ1–MQ7) and 7 Neo4j Cypher queries (CQ1–CQ7), cross-database performance comparison | Query results + performance benchmarks |
| **NB05** | Analysis | Answers RQ1 (Pearson/Spearman correlation), RQ2 (quality dimension ranking), RQ3 (strategy comparison with buy-and-hold baseline) | `analysis_results` collection |

---

## 🔑 Key Technical Lessons

- **py2neo type compatibility:** Neo4j drivers reject `numpy.float64`/`int64` — explicit casting to native Python `float()`/`int()` is required.
- **Sentiment masking:** Using `fillna(0)` on sentiment data inflates coverage metrics. Real coverage must filter for rows where actual news exists.
- **Dynamic detection:** Hardcoding asset names causes silent failures — runtime detection via `distinct()` and `MATCH` queries is more robust.
- **Market regime awareness:** Buy-and-hold (+98%) outperformed all technical signals due to gold's sustained uptrend — a real-world insight, not just an academic result.

---

## 🤖 AI Tools

This project was developed with the assistance of **Claude AI** (Anthropic), as permitted by the course guidelines. Claude was used as a collaborative development partner for architecture design, code development, debugging, data analysis interpretation, and documentation generation. All results and conclusions are derived from real data extracted by the pipeline.

---

## 👤 Author

**Peter Emad Sami Sedik**
- Student ID: 944958
- Università degli Studi di Milano-Bicocca — Master's Degree Program
- Academic Year 2025/2026

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.
