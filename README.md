# poc-mcp-llm-stack

**Proof-of-concept:** an AI architecture combining a Large Language Model (LLM), a Vector Database, and an MCP-style proxy to integrate enterprise systems.  
This project demonstrates how easily AI can be integrated into enterprise workflows as part of the [Infinito.Nexus](https://infinito.nexus/) vision.

---

## 🚀 Installation

Using [Kevin's Package Manager](https://github.com/kevinveenbirkenbach/package-manager):

```bash
pkgmgr install pmls
````

Or clone the repository and start with Docker Compose.

---

## 🧩 Components

* **Ollama + Open WebUI** → run and interact with LLMs
* **LiteLLM** → OpenAI-compatible proxy (MCP-style)
* **Qdrant** → vector database for embeddings & retrieval
* **MinIO** → object storage for documents and knowledge
* **Flowise** → orchestration and experimentation tool
* **Mock APIs** → simulated ERP, CRM, and ticketsystem services

---

## ⚡ Quickstart

### Start the stack

```bash
docker compose up -d
```

### Load model (one-time)

```bash
docker exec -it ollama ollama pull llama3
# optional: embeddings
docker exec -it ollama ollama pull nomic-embed-text
```

---

## 🔗 Access Points

* **Open WebUI** → [http://localhost:3001](http://localhost:3001) (direct Ollama connection)
* **Flowise** → [http://localhost:3000](http://localhost:3000) (login: `admin` / `admin`)
* **Qdrant Console/API** → [http://localhost:6333](http://localhost:6333)
* **LiteLLM (OpenAI API)** → [http://localhost:4000/v1](http://localhost:4000/v1) (API key arbitrary, e.g. `dummy-key`)
* **MinIO Console** → [http://localhost:9001](http://localhost:9001) (`admin` / `adminadmin`)
* **Mock APIs** →

  * Ticketsystem: [http://localhost:7010/\_\_admin](http://localhost:7010/__admin)
  * ERP: [http://localhost:7020](http://localhost:7020)
  * CRM: [http://localhost:7030](http://localhost:7030)

---

## 🛠️ Building Flows in Flowise

* **OpenAI Node**

  * Base URL: `http://litellm:4000/v1`
  * API Key: `dummy-key`
  * Model: `ollama/llama3`

* **Embeddings Node**

  * Base URL: `http://litellm:4000/v1`
  * Model: `ollama/nomic-embed-text`

* **Vector Store Node**

  * URL: `http://qdrant:6333`

* **Tools/HTTP**

  * Call `ticketsystem:8080`, `erp:8080`, `crm:8080` (via container DNS in the same network)

---

## 📚 Knowledge Ingest

1. Upload documents to MinIO (e.g., bucket `knowledge/`).
2. Build a Flowise pipeline: **Loader → Embedder → Qdrant**.

This enables retrieval-augmented generation (RAG) using your documents.
To integrate real systems (e.g., OTRS, ERPNext, Odoo), replace the WireMock containers 1:1 with their official images and update Flowise endpoints.

---

## 👤 Author

Created by **[Kevin Veen-Birkenbach](https://veen.world)**

---

## 🙏 Acknowledgments

* Special thanks to **[Bernd Erk](https://www.netways.de/), CEO of NETWAYS**, for his inspiring talk at the [Bitkom Forum Open Source 2025](https://www.bitkom.org/Forum-Open-Source-2025/).
* Thanks also to **[Bitkom](https://www.bitkom.org/)** for organizing the conference and providing valuable inspiration.

---

## 📖 Context

This project is a PoC within the Infinito.Nexus initiative to evaluate how seamlessly AI can be integrated into modular IT infrastructure.

See the full conversation that led to this project here:
[ChatGPT Log](https://chatgpt.com/share/68ce5e62-7398-800f-8a7a-67e75833b7e9)
