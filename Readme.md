# GENAI Integrated RAG System

RAG_GENAI integrates Google’s Generative AI (Gemini) with a Chroma vector store to build a cloud‑backed Retrieval‑Augmented Generation system. It fetches real arXiv abstracts, indexes them, and uses Gemini to produce citation‑backed answers in a Gradio UI.



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
# Key Features

- **Real abstracts via arxiv API from multiple categories (CS.AI, CV, NLP, Genomics)**
- **Chunking:** 500 char + 50 char overlap for context windows
- **Embeddings:** GoogleGenerativeAIEmbeddings(model="models/text-embedding-004")
- **Vector Store:** Chroma for persistent similarity search
- **Generative LLM:** ChatGoogleGenerativeAI(model="gemini-1.5-pro")
- **RetrievalQA chain via LangChain, enforcing [n] citations**
- **Evaluation:** Precision@K + ROUGE-1 on real abstracts
- **UI:** Gradio Blocks chat with clickable source links

# Dependencies

- **Python ≥ 3.8**
- **langchain**
- **langchain-chroma**
- **langchain-google-generativeai**
- **chromadb**
- **arxiv**
- **gradio**
- **rouge_score**

# Code Structure

- **Cell 1:** Install dependencies (pip install ...)
- **Cell 2:** Imports + prompt for GOOGLE_API_KEY
- **Cell 3:** Fetch real abstracts via arxiv → Document list
- **Cell 4:** Chunk (RecursiveCharacterTextSplitter), embed, Chroma index
- **Cell 5:** retrieve(query, k) wrapper around Chroma.similarity_search
- **Cell 6:** Build RetrievalQA chain with ChatGoogleGenerativeAI
- **Cell 7:** answer_query() helper + quick test printout
- **Cell 8:** Evaluation: Precision@K + ROUGE-1 on test queries
- **Cell 9:** Gradio Blocks UI with chat history & clickable sources

# Challenges & Solutions

- **API Model Names:** Gemini naming changed; we settled on gemini-1.5-pro.
- **Data Coverage:** Listing pages had no abstracts → switched to arxiv package.
- **Integration Complexity:** Abstracted retrieval interface so we can swap vector stores/models.
- **Cost & Latency:** Limited corpus size (≈400 chunks) and reused embeddings to reduce calls.

# Future Work

- **Upgrade to Gemini 2.0 multi-modal capabilities.**
- **Hybrid search:** Combine Chroma + keyword/BM25 reranking.
- **Continuous ingestion:** Sync new arXiv papers automatically.
- **User feedback loop:** Implement a feedback mechanism for on-the-fly fine-tuning.

# Architecture Flow

[User Query]  
↓  
[1] Embed Query (Gemini Embedding API)  
↓  
[2] Chroma vector search → top-k chunks  
↓  
[3] PromptTemplate with citations [1],[2],…  
↓  
[4] ChatGoogleGenerativeAI (Gemini) → Answer  
↓  
[5] Display (Gradio chat + clickable sources)
