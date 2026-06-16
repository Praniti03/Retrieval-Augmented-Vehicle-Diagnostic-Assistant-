```markdown
# 🚗 Retrieval-Augmented Vehicle Diagnostic Assistant

A context-aware chatbot that uses **Retrieval-Augmented Generation (RAG)** to help drivers understand dashboard warning messages — grounded in real car manual data rather than model hallucination.

Built as a proof of concept for integrating LLMs into vehicles, designed to hook into text-to-speech software for hands-free, in-car guidance.

---

## Overview

Modern vehicles display dozens of warning messages that drivers may not recognise or understand. This project ingests pages from a real car manual (MG ZS compact SUV) and builds a RAG pipeline on top of an LLM so that when a driver asks *"What does the Gasoline Particulate Filter Full warning mean?"*, the assistant retrieves the relevant section of the manual and answers accurately — rather than guessing.

---

## How It Works

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

1. The HTML car manual is loaded and parsed using `unstructured`.
2. The text is chunked with `langchain-text-splitters` and embedded into a **Chroma** vector store.
3. On each query, relevant chunks are retrieved and passed as context to the LLM.
4. The LLM (OpenAI, via `langchain-openai`) generates a grounded, accurate response.

---

## Features

- RAG pipeline built with **LangChain**
- Vector storage with **ChromaDB**
- Source document: MG ZS car manual warning messages (`mg-zs-warning-messages.html`)
- Responses grounded in manual content — no hallucinated advice
- Output designed to integrate with text-to-speech for in-vehicle delivery

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

## Getting Started

### Prerequisites

- Python 3.10+
- An OpenAI API key

### Installation

```bash
pip install \
  langchain-core==0.3.72 \
  langchain-openai==0.3.28 \
  langchain-community==0.3.27 \
  langchain-chroma==0.2.5 \
  langchain-text-splitters==0.3.9 \
  unstructured==0.18.11 \
  pydantic==2.11.9
```

### Usage

1. Clone the repository
2. Set your OpenAI API key as an environment variable:
```bash
export OPENAI_API_KEY=your_key_here
```
3. Open and run `notebook.ipynb` cell by cell

---

## Example Query

```
User:      What does the Gasoline Particulate Filter Full warning mean?
Assistant: The Gasoline Particulate Filter Full warning indicates that the
           gasoline particulate filter is full. You should consult an MG
           Authorised Repairer as soon as possible for assistance.
```

---

## Tech Stack

| Component | Library |
|---|---|
| LLM integration | `langchain-openai` |
| Orchestration | `langchain-core`, `langchain-community` |
| Vector store | `langchain-chroma` (ChromaDB) |
| Document parsing | `unstructured` |
| Text splitting | `langchain-text-splitters` |
| Data validation | `pydantic` |

---
Paste that into your GitHub README.md file and it will render perfectly. The triple backtick code blocks, headers, table, and bullet formatting will all display correctly in GitHub's markdown renderer.
