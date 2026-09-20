# 🩺 MediBot — RAG-Based Medical Chatbot

A context-aware medical question-answering assistant built using **Retrieval-Augmented Generation (RAG)**, **FAISS**, **Sentence Transformers**, **LangChain**, and the **Groq API**.

MediBot retrieves relevant information from a local medical knowledge base and uses a Groq-hosted LLM to generate answers grounded in the retrieved context.

> ⚠️ **Disclaimer:** This project is intended for educational and research purposes only. It is not a substitute for professional medical advice, diagnosis, or treatment.

---

## 🧠 Overview

MediBot is a **Retrieval-Augmented Generation (RAG)** application designed to answer medical questions using information retrieved from a curated medical PDF knowledge base.

Instead of relying solely on the LLM's internal knowledge, the system:

1. Loads medical documents from PDF files.
2. Splits the documents into smaller chunks.
3. Converts the chunks into vector embeddings.
4. Stores the embeddings in a **FAISS vector database**.
5. Retrieves the most relevant chunks for a user's question.
6. Sends the retrieved context to a **Groq-hosted LLM**.
7. Generates a context-aware response based on the retrieved information.

This approach helps keep responses grounded in the provided medical reference material.

---

## ✨ Key Features

* 🧠 **Retrieval-Augmented Generation (RAG)**
* 🔎 **Semantic search using FAISS**
* 📚 **PDF-based medical knowledge base**
* 🤖 **Groq API for fast LLM inference**
* 🔤 **Sentence Transformer embeddings**
* 💬 **Interactive Streamlit chatbot interface**
* 📌 **Context-aware responses**
* 📖 **Source document traceability**
* ⚡ **Fast retrieval and inference**
* 💾 **Persistent FAISS vector store**
* 🚀 **Modular LangChain architecture**

---

## 🏗️ System Architecture

```text
                 ┌──────────────────────┐
                 │    Medical PDFs      │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │    Text Extraction   │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │    Text Chunking     │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │ Sentence Transformers│
                 │    Embeddings        │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │    FAISS Vector DB   │
                 └──────────┬───────────┘
                            │
                            │
User Question ──────────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │  Similarity Search   │
                 │      Top-K Chunks    │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │   Prompt Assembly    │
                 │ Query + Context      │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │      Groq API        │
                 │   LLM Inference      │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │  Generated Answer    │
                 │ + Retrieved Sources  │
                 └──────────────────────┘
```

---

## 🛠️ Tech Stack

| Technology                | Purpose                               |
| ------------------------- | ------------------------------------- |
| **Python**                | Core programming language             |
| **LangChain**             | RAG pipeline and LLM orchestration    |
| **Groq API**              | LLM inference                         |
| **FAISS**                 | Vector database and similarity search |
| **Sentence Transformers** | Document embeddings                   |
| **Streamlit**             | Web-based chatbot interface           |
| **PyPDF**                 | PDF document processing               |
| **python-dotenv**         | Environment variable management       |

---

## 📂 Project Structure

```text
medical-chatbot/
│
├── data/
│   └── medical_reference.pdf
│
├── vectorstore/
│   └── db_faiss/
│       ├── index.faiss
│       └── index.pkl
│
├── create_memory_for_llm.py
├── connect_memory_with_llm.py
├── medibot.py
│
├── requirements.txt
├── pyproject.toml
├── .env
├── .gitignore
└── README.md
```

### Main Components

| File / Directory             | Purpose                                                            |
| ---------------------------- | ------------------------------------------------------------------ |
| `create_memory_for_llm.py`   | Loads medical PDFs, creates embeddings, and builds the FAISS index |
| `connect_memory_with_llm.py` | CLI-based RAG implementation                                       |
| `medibot.py`                 | Streamlit chatbot interface using Groq + FAISS                     |
| `vectorstore/db_faiss/`      | Persisted FAISS vector database                                    |
| `data/`                      | Medical PDF knowledge base                                         |
| `.env`                       | Stores API credentials                                             |

---

## 🔐 Environment Variables

The application requires a **Groq API key**.

Create a `.env` file in the project root:

```env
GROQ_API_KEY=your_groq_api_key_here
```

The API key can be obtained from the Groq developer platform.

> 🔒 Never commit your `.env` file or API key to GitHub.

Add the following to `.gitignore`:

```text
.env
.venv/
__pycache__/
*.pyc
```

---

## ⚙️ Installation

### 1. Clone the Repository

```bash
git clone https://github.com/AIwithhassan/medical-chatbot-refactored.git

cd medical-chatbot-refactored
```

### 2. Create a Virtual Environment

#### Windows

```bash
python -m venv .venv
.venv\Scripts\activate
```

#### macOS / Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
```

---

### 3. Install Dependencies

Using pip:

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

Or, if you're using **uv**:

```bash
uv sync
```

---

### 4. Configure the Groq API

Create a `.env` file:

```env
GROQ_API_KEY=your_groq_api_key_here
```

The application loads this key using `python-dotenv`.

---

## 🗂️ Building the FAISS Vector Store

Before running the chatbot, create the vector database from your medical PDF.

Run:

```bash
python create_memory_for_llm.py
```

The script performs the following steps:

```text
Medical PDF
     ↓
Load Documents
     ↓
Split Into Chunks
     ↓
Generate Embeddings
     ↓
Create FAISS Index
     ↓
Save Vector Store
```

The resulting database is stored in:

```text
vectorstore/db_faiss/
```

---

## 🔤 Embedding Model

The project uses the following Sentence Transformer model:

```text
sentence-transformers/all-MiniLM-L6-v2
```

The model converts document chunks into numerical vectors that capture their semantic meaning.

For example:

```text
"Symptoms of diabetes"
```

and

```text
"Common signs associated with diabetes"
```

can be recognized as semantically related even though the wording is different.

These vectors are then stored in FAISS for efficient similarity search.

---

## 🔎 How Retrieval Works

When a user asks a question, the system performs a similarity search against the FAISS database.

For example:

```text
User:
"What are the common symptoms of diabetes?"
```

The retriever searches the vector database and returns the most relevant medical document chunks.

These chunks are then added to the LLM prompt:

```text
Retrieved Context
        +
User Question
        ↓
     Prompt
        ↓
    Groq LLM
        ↓
Context-Grounded Answer
```

---

## 🤖 Groq LLM Integration

Unlike implementations that use Hugging Face inference endpoints, this version uses the **Groq API** for LLM inference.

The Groq API provides access to supported open-source and proprietary language models through a fast inference API.

The general flow is:

```python
User Query
    ↓
FAISS Retriever
    ↓
Relevant Context
    ↓
Prompt Template
    ↓
Groq Chat Model
    ↓
Generated Response
```

The LLM receives the retrieved medical context along with the user's question, allowing the response to be grounded in the project's knowledge base.

---

## 💬 Running the Streamlit Application

After creating the FAISS vector store, start the chatbot:

```bash
streamlit run medibot.py
```

Streamlit will start the application locally.

You can then open the displayed local URL in your browser.

---

## 🧪 Example Interaction

### User

```text
What are the symptoms of anemia?
```

### RAG Pipeline

```text
Question
   ↓
Semantic Search
   ↓
Top-K Relevant Medical Chunks
   ↓
Context + Question
   ↓
Groq LLM
   ↓
Generated Answer
```

### Response

The chatbot generates an answer based primarily on the retrieved information from the medical reference documents.

---

## 📌 Why RAG?

Traditional LLM applications generate answers primarily from the model's learned knowledge.

RAG introduces an additional retrieval step:

```text
Traditional LLM

Question → LLM → Answer
```

Whereas this project uses:

```text
Question
    ↓
Retrieve Relevant Information
    ↓
Add Context
    ↓
Groq LLM
    ↓
Answer
```

This allows the application to use a **custom and updateable knowledge base** without retraining the language model.

---

## 🚀 Future Improvements

Potential improvements include:

* 🔐 User authentication
* 📄 Support for multiple medical documents
* 🗃️ Metadata-based document filtering
* 🔎 Improved hybrid search
* 💬 Conversation memory
* 📊 Retrieval evaluation metrics
* 🧪 RAG evaluation using RAGAS
* 📝 Improved source citations
* 🏥 Structured medical knowledge sources
* ☁️ Cloud deployment
* 📱 Responsive UI
* 🎯 Better prompt engineering
* ⚡ Streaming Groq responses

---

## 🎯 Learning Outcomes

Through this project, I explored:

* Retrieval-Augmented Generation (RAG)
* Vector databases
* Semantic search
* Document embeddings
* FAISS similarity search
* LangChain
* Prompt engineering
* LLM API integration
* Groq inference
* Streamlit application development
* Building AI applications around domain-specific knowledge

---

## ⚠️ Medical Disclaimer

This chatbot is an **educational AI project** and should not be used for medical diagnosis, treatment decisions, emergency situations, or professional healthcare advice.

Always consult a qualified healthcare professional for medical concerns.

---

## 👨‍💻 Author

**Hammad Ansari**

M.Sc. Data Science | AI/ML & Data Analytics Enthusiast

Interested in:

* Data Analytics
* Machine Learning
* Generative AI
* RAG Systems
* LLM Applications
* AI Engineering

---

## ⭐ If You Find This Project Useful

Consider giving the repository a ⭐ on GitHub and exploring the code to understand how RAG applications can be built using local knowledge bases and modern LLM APIs.
