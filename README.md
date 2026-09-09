# InsureClaim — Multi-Embedding RAG Framework for Insurance Policy Analytics

A **Retrieval-Augmented Generation (RAG)** framework for querying insurance policy documents using multiple embedding models, FAISS semantic search, and Gemini-powered answer generation.

InsureClaim allows users to upload policy documents, compare retrieval performance across different embedding models, inspect supporting evidence, and generate grounded answers from retrieved policy content.

---

## Overview

Insurance policies contain large amounts of structured and unstructured information that can be difficult to search manually.

**InsureClaim** addresses this problem by combining:

* Document processing and intelligent text chunking
* Multiple Sentence Transformer embedding models
* FAISS-based vector similarity search
* Evidence-grounded retrieval
* Gemini-powered answer generation
* Retrieval confidence scoring
* Side-by-side embedding model comparison
* Streamlit-based interactive interface

The project focuses on **retrieval quality and evidence grounding**, rather than treating the system as a simple document chatbot.

---

## System Architecture

```text
                 Insurance Policy Documents
                           │
                           ▼
                  Document Processing
                           │
                  PDF / DOCX Extraction
                           │
                           ▼
                     Text Cleaning
                           │
                           ▼
                   Semantic Chunking
                           │
                           ▼
              ┌────────────┼────────────┐
              │            │            │
              ▼            ▼            ▼
        all-mpnet-base-v2  all-MiniLM  multi-qa-mpnet
              │            │            │
              └────────────┼────────────┘
                           ▼
                  FAISS Vector Stores
                           │
                           ▼
                   Semantic Retrieval
                           │
                           ▼
                   Relevant Evidence
                           │
                           ▼
                  Gemini Answer Generator
                           │
                           ▼
              Answer + Evidence + Confidence
                           │
                           ▼
                 Streamlit Application
```

---

## Embedding Models

The framework compares three Sentence Transformer models:

| Model                        | Embedding Dimension | Primary Role                       |
| ---------------------------- | ------------------: | ---------------------------------- |
| `all-mpnet-base-v2`          |                 768 | General-purpose semantic retrieval |
| `all-MiniLM-L6-v2`           |                 384 | Lightweight semantic retrieval     |
| `multi-qa-mpnet-base-dot-v1` |                 768 | Question-answer focused retrieval  |

Each model maintains its own **FAISS vector index**, allowing retrieval results to be compared for the same policy question.

---

## Key Features

### 1. Multi-Format Document Processing

The document processor supports:

* PDF documents
* DOCX documents

The pipeline extracts text, cleans document content, preserves relevant metadata, and divides documents into searchable chunks.

---

### 2. Multi-Embedding Retrieval

Instead of relying on a single embedding model, InsureClaim indexes processed policy content using three different embedding models.

This makes it possible to evaluate how the choice of embedding model affects semantic retrieval.

```text
Policy Documents
      ↓
Text Chunks
      ↓
┌───────────────┬───────────────┬──────────────────┐
│ MPNet         │ MiniLM        │ Multi-QA MPNet   │
└───────────────┴───────────────┴──────────────────┘
      ↓
Independent FAISS Vector Stores
      ↓
Retrieval Comparison
```

---

### 3. FAISS Semantic Search

FAISS is used to perform efficient vector similarity search over generated document embeddings.

For each user query:

```text
User Question
      ↓
Query Embedding
      ↓
FAISS Similarity Search
      ↓
Top Relevant Chunks
```

---

### 4. Evidence-Grounded Answer Generation

Retrieved policy passages are provided to the Gemini-based answer generator as evidence.

The generated response includes:

* Answer
* Supporting evidence
* Source information
* Confidence estimation

This helps keep responses grounded in the uploaded policy documents instead of relying solely on the language model's internal knowledge.

---

### 5. Embedding Model Comparison

The Streamlit application can process the same question through all embedding models and compare their retrieval behavior.

The interface provides:

* Retrieved answers
* Confidence scores
* Supporting evidence
* Retrieval comparison
* Interactive confidence visualization

This allows users to inspect how different embedding models affect the downstream RAG pipeline.

---

## Retrieval Workflow

For each query, the system follows:

```text
Question
   ↓
Query Embedding
   ↓
FAISS Similarity Search
   ↓
Relevant Policy Chunks
   ↓
Evidence Selection
   ↓
Gemini Answer Generation
   ↓
Confidence + Sources
```

When comparing models, the same question is processed independently through each embedding model, allowing their retrieval and answer characteristics to be inspected side by side.

---

## Project Structure

```text
InsureClaim-Multi-Embedding-RAG/
│
├── app_multi_embedding.py
├── answer_generator.py
├── document_processor.py
├── vector_store.py
├── generate_gold_dataset.py
├── gold_test_dataset.json
├── requirements.txt
├── .gitignore
│
└── sample_data/
    ├── sample_policy_1.pdf
    └── sample_policy_2.pdf
```
---

## Core Components

### `app_multi_embedding.py`

Main Streamlit application.

Responsible for:

* Initializing embedding systems
* Uploading policy documents
* Processing and indexing documents
* Querying individual embedding models
* Comparing all embedding models
* Displaying retrieved evidence and confidence
* Rendering the interactive UI

---

### `document_processor.py`

Handles document ingestion and preprocessing.

Responsibilities include:

* PDF extraction
* DOCX extraction
* Text cleaning
* Chunk generation
* Document metadata handling

---

### `vector_store.py`

Implements the semantic retrieval layer.

Contains components for:

* Sentence Transformer embeddings
* FAISS vector indexing
* Similarity search
* Metadata storage
* Semantic retrieval

---

### `answer_generator.py`

Handles Gemini-powered answer generation.

The component:

* Builds evidence-grounded prompts
* Generates answers from retrieved context
* Extracts supporting evidence
* Tracks sources
* Calculates confidence-related signals

---

### `generate_gold_dataset.py`

Creates curated evaluation questions and reference answers for the policy documents.

The dataset contains information such as:

* Questions
* Ground-truth answers
* Source sections
* Difficulty
* Expected retrieval chunks

---

### `gold_test_dataset.json`

Curated evaluation dataset containing reference questions and answers for testing the RAG pipeline.

---

## Evaluation Dataset

The project includes a curated **gold test dataset** containing policy-related questions and reference answers.

This provides a foundation for evaluating:

* Retrieval quality
* Answer correctness
* Evidence relevance
* Embedding model differences

The dataset is included as:

```text
gold_test_dataset.json
```

---

## Technologies Used

### AI & Retrieval

* Python
* Sentence Transformers
* FAISS
* PyTorch
* Google Gemini

### Document Processing

* PyMuPDF
* python-docx
* NLTK

### Application & Visualization

* Streamlit
* Plotly

---

## Installation

### Prerequisites

* Python 3.9+
* Google Gemini API key

### 1. Clone the Repository

```bash
git clone https://github.com/<your-username>/InsureClaim-Multi-Embedding-RAG.git
cd InsureClaim-Multi-Embedding-RAG
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

---

## Gemini API Configuration

The application requires a Google Gemini API key.

Set the key as an environment variable rather than placing it directly in the source code.

### Windows CMD

```cmd
set GOOGLE_API_KEY=YOUR_API_KEY
```

### PowerShell

```powershell
$env:GOOGLE_API_KEY="YOUR_API_KEY"
```

> **Security:** Never commit API keys, `.env` files, credentials, or other secrets to GitHub.

---

## Running the Application

Start the Streamlit application:

```bash
streamlit run app_multi_embedding.py
```

The application will open in your browser.

Upload an insurance policy document and enter a question related to its contents.

The system will retrieve relevant policy passages and generate an evidence-grounded response.

---

## Example Questions

InsureClaim can be used for questions such as:

* What conditions are covered under this policy?
* What exclusions are mentioned in the policy?
* What documents are required to submit a claim?
* What is the waiting period?
* What are the eligibility requirements?
* What are the claim settlement conditions?

---

## Why Multi-Embedding RAG?

Embedding models represent text differently.

A model optimized for general semantic similarity may retrieve different passages from a model designed specifically for question-answering.

By maintaining independent vector stores for multiple embedding models, InsureClaim makes these differences observable rather than hiding them behind a single retrieval configuration.

This provides a practical framework for studying the relationship between:

```text
Embedding Model
      ↓
Semantic Retrieval
      ↓
Retrieved Evidence
      ↓
Generated Answer
      ↓
Answer Confidence
```

---

## Security

API credentials are loaded through the `GOOGLE_API_KEY` environment variable.

Credentials should never be:

* Hardcoded into source files
* Committed to version control
* Included in public datasets
* Stored in the repository

The repository should also exclude sensitive environment files and generated credentials through `.gitignore`.

---

## Future Improvements

Potential extensions include:

* Hybrid BM25 + vector retrieval
* Cross-encoder reranking
* Automated retrieval evaluation
* Better confidence calibration
* Citation-level answer verification
* Persistent vector-store management
* Support for additional embedding models
* Deployment as a cloud-hosted application

---

## Project Highlights

InsureClaim demonstrates practical implementation of:

**Retrieval-Augmented Generation · Semantic Search · Vector Databases · Embedding Model Evaluation · Evidence Grounding · Generative AI · Document Intelligence · Interactive AI Applications**

---

## Repository Notes

This project was developed as an applied **Generative AI and NLP portfolio project**, with an emphasis on understanding how embedding-model selection influences retrieval quality and downstream RAG performance.

Rather than treating RAG as a black-box chatbot, InsureClaim exposes the retrieval layer and makes the differences between embedding strategies observable and comparable.
