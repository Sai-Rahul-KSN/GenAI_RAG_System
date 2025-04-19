# Open AI Integrated RAG System

This repository contains two separate Retrieval-Augmented Generation (RAG) projects demonstrating end-to-end pipelines for academic literature question answering using different LLM backends and corpora:

## **GenAI_RAG**:
Gemini-powered RAG system using Google Generative AI embeddings and LLM via LangChain.
## Introduction

A Gemini-based RAG pipeline using `GoogleGenerativeAIEmbeddings` and `ChatGoogleGenerativeAI` (Gemini-1.5-Pro), indexing real arXiv abstracts via the arxiv API.

---

### Requirements

- Python 3.8+  
- Google Cloud API key with GenAI access  
- Dependencies: see `GenAI_RAG/requirements.txt`

### Installation & Setup
```bash
git clone https://github.com/Sai-Rahul-KSN/RAG_PROJECT.git
cd RAG_PROJECT.git
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt
export OPENAI_API_KEY="..."  # your OpenAI key 
```
### Building the Index

```bash
python scripts/load_arxiv_genai.py      #  fetch and chunk abstracts
python scripts/build_chromadb.py     # embed & index with Chroma
```
### Running the System

**CLI:**
```bash
python scripts/run_genai_rag.py --query "Explain Transformer architecture" --k 4
```
### Web UI:
``` bash
cd ui
python genai_app.py
# then visit the displayed URL
```
