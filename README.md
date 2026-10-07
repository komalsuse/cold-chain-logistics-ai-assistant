# 🧊 Cold-Chain Logistics AI-Assistant

A multi-tool AI agent that lets logistics teams query cold-chain operations in plain English. It combines **SQL fleet data**, **RAG-based SOP/compliance document retrieval**, and **live weather data** behind one conversational interface, built with security and auditability in mind.

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![LangGraph](https://img.shields.io/badge/LangGraph-agent-green)
![Docker](https://img.shields.io/badge/Docker-containerized-2496ED)
![AWS](https://img.shields.io/badge/AWS-EC2-orange)

---

## ✨ Features

- **Multi-tool agent (LangGraph)**: routes each question to the right tool: SQL, document retrieval, or weather.
- **Natural-language SQL querying**: ask questions about fleet data without writing queries.
- **RAG over SOPs and compliance documents**: semantic search using Pinecone, with answers grounded in the source documents.
- **Live weather integration**: real-time weather lookups to support route and temperature-risk decisions.
- **Enterprise-grade security**
  - Least-privilege database roles
  - Semantic views to expose only what the agent needs
  - Immutable audit logging of agent actions
- **Incremental ingestion pipeline**: hash-based change detection, so only new or modified multi-format documents are re-processed.
- **Containerized deployment**: Dockerized MSSQL and app, deployed on AWS EC2.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Language | Python |
| Agent orchestration | LangGraph, LangChain |
| LLM | DeepSeek |
| Vector store | Pinecone |
| Database | Microsoft SQL Server (MSSQL) |
| UI | Streamlit |
| Deployment | Docker, AWS EC2 |

---

## 🏗️ Architecture

```
          ┌──────────────┐
          │  Streamlit   │
          │      UI      │
          └──────┬───────┘
                 │
          ┌──────▼───────┐
          │  LangGraph   │
          │    Agent     │
          └──┬────┬────┬─┘
             │    │    │
   ┌─────────▼┐ ┌─▼────────┐ ┌▼──────────┐
   │ SQL Tool │ │ RAG Tool │ │ Weather   │
   │ (MSSQL)  │ │(Pinecone)│ │ API Tool  │
   └─────┬────┘ └────▲─────┘ └───────────┘
         │           │
 Semantic views   Ingestion pipeline
 + audit log      (hash-based change detection)
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10+
- Docker and Docker Compose
- API keys for DeepSeek, Pinecone, and your weather provider

### 1. Clone the repository

```bash
git clone https://github.com/komalsuse/<repo-name>.git
cd <repo-name>
```

### 2. Configure environment variables

Create a `.env` file in the project root (adjust names to match your code):

```env
DEEPSEEK_API_KEY=your_key
PINECONE_API_KEY=your_key
PINECONE_INDEX=your_index_name
WEATHER_API_KEY=your_key
MSSQL_SERVER=localhost
MSSQL_DATABASE=your_db
MSSQL_USER=agent_readonly_user
MSSQL_PASSWORD=your_password
```

> ⚠️ Never commit your `.env` file. Add it to `.gitignore`.

### 3. Start the database

```bash
docker compose up -d
```

### 4. Install dependencies

```bash
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 5. Ingest documents

Place your SOP/compliance documents in the documents folder, then run the ingestion script. Only new or changed files are processed.

```bash
python <ingestion_script>.py
```

### 6. Run the app

```bash
streamlit run app.py
```

---

## 💬 Example Queries

- *"Which vehicles in the fleet had a temperature excursion this week?"*
- *"What does our SOP say about handling a refrigeration failure during transit?"*
- *"Will weather along the Pune–Mumbai route affect today's vaccine shipment?"*

---

## 🔐 Security Design

- The agent connects with a **least-privilege DB role**: read-only access, no direct table access.
- Data is exposed through **semantic views** rather than raw tables.
- Every agent action is written to an **immutable audit log** for traceability.

---

## ☁️ Deployment

The application and MSSQL run as Docker containers on an **AWS EC2** instance.

1. Launch an EC2 instance and install Docker.
2. Copy the project and `.env` to the server.
3. Run `docker compose up -d`.
4. Open the Streamlit port in the instance's security group.

---

## 🗺️ Roadmap

- [ ] Alerting for temperature breaches
- [ ] Role-based access in the UI
- [ ] Evaluation suite for agent accuracy

---

## 👩‍💻 Author

**Komal Suse**
[LinkedIn](https://linkedin.com/in/komalsuse) · [GitHub](https://github.com/komalsuse) · susekomal@gmail.com

---

## 📄 License

This project is licensed under the MIT License. See the `LICENSE` file for details.
