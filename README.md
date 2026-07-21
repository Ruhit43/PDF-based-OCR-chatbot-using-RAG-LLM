# 📄 PDF Knowledge Chatbot — Local LLM RAG System

A fully local, completely free PDF question-answering chatbot powered by Mistral 7B. Upload any PDF documents, ask questions in natural language, and get accurate grounded answers — no API keys, no internet after setup, no cost.

---

## 🧠 How It Works

Built on the RAG (Retrieval-Augmented Generation) architecture:

1. **Extract** — reads and cleans text from multiple PDFs using PyMuPDF
2. **Chunk** — splits text into overlapping word windows to preserve context at boundaries
3. **Embed** — converts every chunk into a 384-dimensional semantic vector using Sentence Transformers (`all-MiniLM-L6-v2`)
4. **Retrieve** — on each question, finds the most relevant chunks using cosine similarity
5. **Gate** — rejects questions scoring below a relevance threshold so the LLM never hallucinates
6. **Generate** — sends retrieved chunks + question to Mistral 7B running locally via Ollama and returns a natural language answer with source citations

```
PDF Files → Extract → Chunk → Embed → Store in Memory
                                          ↓
User Question → Embed → Cosine Similarity → Threshold Check
                                          ↓
                              Build Prompt → Mistral 7B → Answer
```

---

## ✨ Key Features

- Works with any PDF — no hardcoded content
- Supports multiple PDFs simultaneously as one knowledge base
- Correctly refuses out-of-domain questions ("not in my knowledge base")
- Handles paraphrased questions through semantic understanding
- Cites which document and page each answer came from
- Resistant to prompt injection attacks
- 100% offline after initial model download

---

## 🛠️ Tech Stack

| Component | Library |
|---|---|
| PDF extraction | PyMuPDF (fitz) |
| Text embeddings | Sentence Transformers |
| Similarity search | scikit-learn cosine similarity |
| Local LLM | Mistral 7B via Ollama |
| Notebook environment | Jupyter Notebook |
| Language | Python 3.x |

---

## ⚙️ Installation & Setup

### Step 1 — Install Python
Download from [python.org](https://www.python.org/downloads/) and check **"Add Python to PATH"** during installation.

### Step 2 — Install Ollama
Download from [ollama.com/download](https://ollama.com/download) and run the installer. On Windows it starts automatically as a background service.

### Step 3 — Download Mistral
```bash
ollama pull mistral
```
~4GB download, one time only.

### Step 4 — Install Python libraries
```bash
pip install pymupdf sentence-transformers scikit-learn requests numpy
```

### Step 5 — Launch Jupyter
```bash
jupyter notebook
```

---

## 🚀 Usage

1. Open `PDF_QA_Approach5_LocalLLM.ipynb` in Jupyter
2. Run **Cell 4** to verify Ollama is working
3. Run **Cell 5** — a file picker opens, select your PDF files
4. Run **Cell 6** — extracts and chunks all PDF text
5. Run **Cell 7** — builds the semantic retrieval index
6. Run **Cell 8** — tests the full RAG pipeline
7. Run **Cell 9** — start chatting with your PDFs

---

## 📊 Performance (tested on mental health PDFs)

| Query Type | Result |
|---|---|
| Direct questions | ✅ Accurate, sourced answers |
| Paraphrased questions | ✅ Correctly matched via semantics |
| Multi-document questions | ✅ Synthesized across sources |
| Out-of-domain questions | ✅ Correctly rejected |
| Prompt injection attempts | ✅ Blocked at retrieval stage |

---

## 📁 Project Structure

```
PDF_QA/
│
├── PDF_QA_Approach5_LocalLLM.ipynb   ← main notebook
├── README.md                          ← this file
└── your_pdfs/                         ← put your PDFs here
```

---

## 🔧 Configuration

Inside the notebook you can adjust:

| Parameter | Default | Effect |
|---|---|---|
| `SIMILARITY_THRESHOLD` | 0.25 | Minimum score to attempt an answer |
| `TOP_K` | 4 | Number of chunks sent to LLM |
| `chunk_size` | 200 | Words per chunk |
| `overlap` | 40 | Shared words between chunks |
| `temperature` | 0.2 | LLM creativity (lower = more factual) |

---

## 📋 Requirements

- Python 3.8+
- Ollama with Mistral pulled
- 8GB+ RAM recommended
- ~5GB disk space for Mistral model

---

## 👤 Author

**Ruhit Ahmed Rizon**
MSc Artificial Intelligence — Dublin Business School
BSc Computer Science & Engineering — BRAC University

---

## 📄 License

MIT License — free to use, modify, and distribute.
