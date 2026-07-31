# Architecture Diagram

This is a simple architecture diagram for the RAG multi-model application. It shows how clients interact with the FastAPI app, and how the app integrates retrieval, embedding, LLMs, and optional caching or storage components.

```mermaid
flowchart LR
  %% Clients
  Browser[Browser / CLI]
  Mobile[Mobile / Other Clients]

  %% Server
  Browser -->|HTTP| Uvicorn[Uvicorn (ASGI)]
  Mobile -->|HTTP| Uvicorn
  Uvicorn --> FastAPI[FastAPI application]

  %% FastAPI responsibilities
  FastAPI -->|retrieve docs| Retriever[Retrieval Layer]
  FastAPI -->|embed text| Embedding[Embedding Service]
  FastAPI -->|call| LLM[LLM(s) / OpenAI API]
  FastAPI -->|serve| Static[Templates / Static files]
  FastAPI -->|cache| Cache[Redis (optional)]

  %% Retrieval internals
  Retriever --> VectorDB[(Vector DB / FAISS / Milvus)]
  Retriever --> DocStore[(Document Storage / S3)]

  %% Embeddings and LLMs
  Embedding --> EmbeddingModel[(Local or Hosted Embedding Model)]
  LLM -->|generate| Output[Response]
  LLM -->|use context| Retriever

  %% Data flows
  VectorDB --> Retriever
  DocStore --> Retriever
  Output --> FastAPI
  FastAPI -->|HTTP response| Browser

  classDef infra fill:#f8f9fa,stroke:#333,stroke-width:1px;
  class Uvicorn,FastAPI,VectorDB,DocStore,Cache,EmbeddingModel,LLM infra;

  %% Notes
  subgraph Notes[ ]
    note1[(Optional: background workers for indexing, ingestion, and model orchestration)]
  end
  note1 --- VectorDB
  note1 --- DocStore
```

Notes

- This diagram is intentionally high-level; concrete components (e.g., which vector DB, which LLMs) depend on your deployment choices.
- The app can run locally via `uvicorn app:app --reload --host 0.0.0.0 --port 8000` and the diagram can be previewed in VS Code using a Mermaid preview extension.

File: docs/architecture.md

