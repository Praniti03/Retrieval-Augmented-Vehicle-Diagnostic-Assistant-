# 🚗 Retrieval-Augmented Vehicle Diagnostic Assistant

A context-aware chatbot that uses **Retrieval-Augmented Generation (RAG)** to help drivers understand dashboard warning messages — grounded in real car manual data rather than model hallucination.

Built as a proof of concept for integrating LLMs into vehicles, designed to hook into text-to-speech software for hands-free, in-car guidance.

---

## Overview

Modern vehicles display dozens of warning messages that drivers may not recognise or understand. This project ingests pages from a real car manual (MG ZS compact SUV) and builds a RAG pipeline on top of an LLM so that when a driver asks *"What does the Gasoline Particulate Filter Full warning mean?"*, the assistant retrieves the relevant section of the manual and answers accurately — rather than guessing.

**Research Questions:**
- Can an LLM grounded in a real car manual answer warning message queries accurately?
- How effectively does RAG prevent hallucinated or generic automotive advice?
- Can the output quality support real-time, text-to-speech in-vehicle delivery?

---

## Dataset / Source Document

| File | Type | Description |
|---|---|---|
| `mg-zs-warning-messages.html` | HTML | MG ZS car manual — warning messages section |

- **Vehicle:** MG ZS Compact SUV
- **Content:** Dashboard warning symbols, meanings, severity levels, and recommended driver actions
- **No missing or incomplete entries** — full manual section used as-is

---

## Project Structure

```
├── notebook.ipynb                  # Main notebook — full RAG pipeline
├── mg-zs-warning-messages.html     # Source document (car manual excerpt)
├── images/
│   └── dashboard.jpg               # Illustrative dashboard image
└── README.md
```

---

## Installation

```bash
# Clone the repository
git clone https://github.com/<your-username>/rag-vehicle-diagnostic-assistant.git
cd rag-vehicle-diagnostic-assistant

# Install dependencies
pip install \
  langchain-core==0.3.72 \
  langchain-openai==0.3.28 \
  langchain-community==0.3.27 \
  langchain-chroma==0.2.5 \
  langchain-text-splitters==0.3.9 \
  unstructured==0.18.11 \
  pydantic==2.11.9
```

Then open the notebook:

```bash
jupyter notebook notebook.ipynb
```

---

## Methodology

### Document Ingestion
- HTML manual loaded and parsed using `unstructured`
- Cleaned and structured into processable text chunks

### Chunking & Embedding
- Text split using `langchain-text-splitters` with overlap to preserve context across chunk boundaries
- Chunks embedded and stored in a **Chroma** vector store for semantic retrieval

### Retrieval-Augmented Generation
- On each user query, the top-k most relevant chunks are retrieved from the vector store
- Retrieved context is injected into the LLM prompt alongside the user question
- OpenAI LLM (via `langchain-openai`) generates a grounded, manual-backed response

### Pipeline

```
Car Manual (HTML)
      │
      ▼
Document Loader (unstructured)
      │
      ▼
Text Splitter  ──►  Chroma Vector Store
                           │
                    Semantic Retrieval
                           │
              User Query ──┤
                           ▼
                  OpenAI LLM (via LangChain)
                           │
                           ▼
                 Grounded Natural Language Answer
```

---

## Example Query

```
User:      What does the Gasoline Particulate Filter Full warning mean?


## Tech Stack

| Component | Library / Tool |
|---|---|
| LLM | OpenAI GPT (via `langchain-openai`) |
| Orchestration | `langchain-core`, `langchain-community` |
| Vector Store | `langchain-chroma` (ChromaDB) |
| Document Parsing | `unstructured` |
| Text Splitting | `langchain-text-splitters` |
| Data Validation | `pydantic` |
| Notebook Environment | Jupyter Notebook |
| Language | Python 3.10+ |
