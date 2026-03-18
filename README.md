```
███╗   ███╗███████╗██████╗ ██╗ ██████╗ █████╗ ██╗
████╗ ████║██╔════╝██╔══██╗██║██╔════╝██╔══██╗██║
██╔████╔██║█████╗  ██║  ██║██║██║     ███████║██║
██║╚██╔╝██║██╔══╝  ██║  ██║██║██║     ██╔══██║██║
██║ ╚═╝ ██║███████╗██████╔╝██║╚██████╗██║  ██║███████╗
╚═╝     ╚═╝╚══════╝╚═════╝ ╚═╝ ╚═════╝╚═╝  ╚═╝╚══════╝
         A S S I S T A N T
```

### *Where Medical Knowledge Meets Generative Intelligence*

---

## 🧬 What is Medical Assistant?

> **Stop hallucinating. Start knowing.**

Medical Assistant is a domain-specific AI chatbot that lets you **talk to your medical documents**. Upload PDFs — textbooks, clinical reports, research papers — and ask questions in plain English. The system retrieves the most relevant context before generating answers, ensuring responses are always **grounded in your documents**, never fabricated.

Built on the **RAG (Retrieval-Augmented Generation)** paradigm, it bridges the gap between raw document storage and intelligent, conversational access to medical knowledge.

---

## ✨ Key Features

| Feature | Description |
|--------|-------------|
| 📄 **PDF Ingestion** | Upload one or multiple medical PDFs via a simple UI |
| 🔍 **Semantic Search** | Chunks are embedded and retrieved by meaning, not keywords |
| 🧠 **GPT-4o Powered** | OpenAI's most capable model for nuanced medical Q&A |
| 📌 **Pinecone Vector DB** | Blazing-fast nearest-neighbour retrieval at scale |
| 🚫 **Anti-Hallucination** | RAG architecture grounds every answer in source documents |
| ⚡ **FastAPI Backend** | Clean REST API with dedicated upload and Q&A endpoints |
| 🎛️ **Streamlit Frontend** | Intuitive chat interface with history download support |
| ☁️ **Cloud Deployed** | Backend on Render, frontend on Streamlit Cloud |

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        USER INTERFACE                           │
│                    (Streamlit Frontend)                         │
└───────────────────────────┬─────────────────────────────────────┘
                            │ HTTP
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                      FASTAPI BACKEND                            │
│                                                                 │
│   POST /upload_pdfs/          POST /ask/                       │
│         │                          │                           │
│         ▼                          ▼                           │
│   ┌──────────────┐        ┌──────────────────┐                │
│   │  PDF Loader  │        │  Query Handler   │                │
│   │  + Chunker   │        │  (RAG Chain)     │                │
│   └──────┬───────┘        └──────┬───────────┘                │
│          │                       │                             │
│          ▼                       ▼                             │
│   ┌──────────────┐        ┌──────────────────┐                │
│   │  OpenAI      │        │  Pinecone        │                │
│   │  Embeddings  │───────▶│  Vector Store    │                │
│   └──────────────┘        └──────────────────┘                │
│                                   │                             │
│                                   ▼                             │
│                          ┌──────────────────┐                  │
│                          │   OpenAI GPT-4o  │                  │
│                          │   (LangChain)    │                  │
│                          └──────────────────┘                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

```
┌─────────────┬───────────────────────────────────────┐
│ Layer        │ Technology                            │
├─────────────┼───────────────────────────────────────┤
│ LLM          │ OpenAI GPT-4o                        │
│ Embeddings   │ OpenAI text-embedding-3-small        │
│ Vector DB    │ Pinecone                             │
│ Orchestration│ LangChain                            │
│ Backend      │ FastAPI + Uvicorn                    │
│ Frontend     │ Streamlit                            │
│ Deployment   │ Render (API) + Streamlit Cloud (UI)  │
│ Language     │ Python 3.11+                         │
└─────────────┴───────────────────────────────────────┘
```

---

## ⚡ Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/snsupratim/medicalAssistant.git
cd medicalAssistant
```

### 2. Setup & Run the Backend

```bash
cd server

# Create and activate virtual environment
uv venv
source .venv/bin/activate       # Windows: .venv\Scripts\activate

# Install dependencies
uv pip install -r requirements.txt
```

Create a `.env` file in `/server`:

```env
OPENAI_API_KEY=sk-...
PINECONE_API_KEY=...
```

```bash
# Start the server
uvicorn main:app --reload --port 8000
```

### 3. Setup & Run the Frontend

```bash
cd ../client

uv venv
source .venv/bin/activate
uv pip install -r requirements.txt

streamlit run app.py
```

### 4. Use It!

- Open `http://localhost:8501` in your browser
- Upload a medical PDF (e.g., a diabetes textbook)
- Ask questions like *"What are the symptoms of Type 2 Diabetes?"*
- Get precise, document-grounded answers instantly ✅

---

## 📡 API Reference

### `POST /upload_pdfs/`
Upload one or more PDF files to be embedded and stored in Pinecone.

```bash
curl -X POST "http://localhost:8000/upload_pdfs/" \
  -F "files=@diabetes_textbook.pdf"
```

**Response:**
```json
{ "message": "2 PDF(s) uploaded and indexed successfully." }
```

---

### `POST /ask/`
Ask a question against the uploaded documents.

```bash
curl -X POST "http://localhost:8000/ask/" \
  -F "question=What is insulin resistance?"
```

**Response:**
```json
{
  "answer": "Insulin resistance is a condition where cells in the body...",
  "sources": ["diabetes_textbook.pdf - Page 47"]
}
```

---

## 📁 Project Structure

```
medicalAssistant/
│
├── 📁 assets/
│   ├── medicalAssistant.png        # Project thumbnail
│   └── MedicalAssistant.pdf        # Architecture diagram
│
├── 📁 client/                      # Streamlit Frontend
│   ├── app.py                      # Main Streamlit app
│   ├── config.py                   # Config & constants
│   ├── components/
│   │   ├── chatUI.py               # Chat interface
│   │   ├── upload.py               # PDF uploader
│   │   └── history_download.py     # Download chat history
│   └── utils/
│       └── api.py                  # Backend API calls
│
├── 📁 server/                      # FastAPI Backend
│   ├── main.py                     # App entry point
│   ├── routes/
│   │   ├── upload_pdfs.py          # PDF upload route
│   │   └── ask_question.py         # Q&A route
│   ├── modules/
│   │   ├── llm.py                  # LLM setup (OpenAI/LangChain)
│   │   ├── load_vectorstore.py     # Pinecone connection
│   │   ├── pdf_handlers.py         # PDF parsing & chunking
│   │   └── query_handlers.py       # RAG chain logic
│   └── middlewares/
│       └── exception_handlers.py   # Global error handling
│
├── main.py
└── README.md
```

---

## 🚀 Deployment

### Backend (Render)

1. Connect your GitHub repo to [Render](https://render.com)
2. Set **Start Command**:
```bash
uvicorn main:app --host 0.0.0.0 --port 10000
```
3. Add environment variables: `OPENAI_API_KEY`, `PINECONE_API_KEY`

### Frontend (Streamlit Cloud)

1. Connect repo to [Streamlit Cloud](https://streamlit.io/cloud)
2. Set main file: `client/app.py`
3. Add `BACKEND_URL` secret pointing to your Render URL

---

## 🔮 What's Next

- [ ] 🔐 User authentication & personal document vaults
- [ ] 🌍 Multi-language support for regional medical documents
- [ ] 📊 Source citation with page numbers in responses
- [ ] 🧪 Symptom checker mode with structured output
- [ ] 📱 Mobile-responsive UI

---

## 👨‍💻 Author

**Built with ❤️ by [BHARATH D R]**

*Inspired by the LangChain, OpenAI, Pinecone, and FastAPI ecosystems*
