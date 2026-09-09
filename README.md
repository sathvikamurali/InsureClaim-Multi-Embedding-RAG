\# InsureClaim — Multi-Embedding RAG Framework for Insurance Policy Analytics



A Retrieval-Augmented Generation (RAG) framework for querying insurance policy documents using multiple embedding models, FAISS semantic search, and Gemini-powered answer generation.



The system allows users to upload policy documents, compare retrieval performance across different embedding models, inspect supporting evidence, and generate grounded answers from the retrieved policy content.



\---



\## Overview



Insurance policies contain large amounts of structured and unstructured information that can be difficult to search manually.



InsureClaim addresses this problem by combining:



\- Document processing and intelligent text chunking

\- Multiple Sentence Transformer embedding models

\- FAISS-based vector similarity search

\- Evidence-grounded retrieval

\- Gemini-powered answer generation

\- Retrieval confidence scoring

\- Side-by-side embedding model comparison

\- Streamlit-based interactive interface



The project focuses on \*\*retrieval quality and evidence grounding\*\*, rather than treating the system as a simple document chatbot.



\---



\## System Architecture



```text

&#x20;                   Insurance Policy Documents

&#x20;                             │

&#x20;                             ▼

&#x20;                   Document Processing

&#x20;                             │

&#x20;                   PDF / DOCX Extraction

&#x20;                             │

&#x20;                             ▼

&#x20;                      Text Cleaning

&#x20;                             │

&#x20;                             ▼

&#x20;                    Semantic Chunking

&#x20;                             │

&#x20;                             ▼

&#x20;               ┌─────────────┼─────────────┐

&#x20;               │             │             │

&#x20;               ▼             ▼             ▼

&#x20;         all-mpnet-base-v2  all-MiniLM   multi-qa-mpnet

&#x20;                             │

&#x20;                             ▼

&#x20;                    FAISS Vector Stores

&#x20;                             │

&#x20;                             ▼

&#x20;                      Semantic Retrieval

&#x20;                             │

&#x20;                             ▼

&#x20;                    Relevant Evidence

&#x20;                             │

&#x20;                             ▼

&#x20;                   Gemini Answer Generator

&#x20;                             │

&#x20;                             ▼

&#x20;                 Answer + Evidence + Confidence

&#x20;                             │

&#x20;                             ▼

&#x20;                   Streamlit Application

Embedding Models



The framework compares three Sentence Transformer models:



Model	Embedding Dimension	Primary Role

all-mpnet-base-v2	768	General-purpose semantic retrieval

all-MiniLM-L6-v2	384	Lightweight semantic retrieval

multi-qa-mpnet-base-dot-v1	768	Question-answer focused retrieval



Each model maintains its own FAISS vector index, allowing retrieval results to be compared for the same policy question.



Key Features

1\. Multi-Format Document Processing



The document processor supports:



PDF documents

DOCX documents



The pipeline extracts text, cleans document content, preserves relevant metadata, and divides documents into searchable chunks.



2\. Multi-Embedding Retrieval



Instead of relying on a single embedding model, InsureClaim indexes the processed policy content using three different embedding models.



This makes it possible to evaluate how embedding choice affects semantic retrieval.



3\. FAISS Semantic Search



FAISS is used to perform efficient vector similarity search over the generated document embeddings.



For a user query:



User Question

&#x20;     ↓

Query Embedding

&#x20;     ↓

FAISS Similarity Search

&#x20;     ↓

Top Relevant Chunks

4\. Evidence-Grounded Answer Generation



Retrieved policy passages are provided to the Gemini-based answer generator as evidence.



The generated response includes:



Answer

Supporting evidence

Source information

Confidence estimation



This helps keep responses grounded in the uploaded policy documents.



5\. Embedding Model Comparison



The Streamlit application can query all embedding models for the same question and compare their results.



The interface provides:



Retrieved answers

Confidence scores

Supporting evidence

Retrieval comparison

Interactive confidence visualization

Project Structure

InsureClaim-Multi-Embedding-RAG/

│

├── app\_multi\_embedding.py

├── answer\_generator.py

├── document\_processor.py

├── vector\_store.py

├── generate\_gold\_dataset.py

├── gold\_test\_dataset.json

├── requirements.txt

├── .gitignore

│

├── sample\_data/

│   ├── sample\_policy\_1.pdf

│   └── sample\_policy\_2.pdf

│

└── archive/

&#x20;   └── Experimental development files

Core Components

app\_multi\_embedding.py



Main Streamlit application.



Responsible for:



Initializing embedding systems

Uploading policy documents

Processing and indexing documents

Querying individual embedding models

Comparing all embedding models

Displaying retrieved evidence and confidence

Rendering the interactive UI

document\_processor.py



Handles document ingestion and preprocessing.



Responsibilities include:



PDF extraction

DOCX extraction

Text cleaning

Chunk generation

Document metadata handling

vector\_store.py



Implements the semantic retrieval layer.



Contains components for:



Sentence Transformer embeddings

FAISS vector indexing

Similarity search

Metadata storage

Semantic retrieval

answer\_generator.py



Handles Gemini-powered answer generation.



The component:



Builds evidence-grounded prompts

Generates answers from retrieved context

Extracts supporting evidence

Tracks sources

Calculates confidence-related signals

generate\_gold\_dataset.py



Creates curated evaluation questions and reference answers for the policy documents.



The dataset contains information such as:



Questions

Ground-truth answers

Source sections

Difficulty

Expected retrieval chunks

gold\_test\_dataset.json



Curated evaluation dataset used to provide reference questions and answers for testing the RAG pipeline.



Technologies Used

Python

Streamlit

Sentence Transformers

FAISS

PyTorch

Google Gemini

PyMuPDF

python-docx

Plotly

NLTK

Installation



Clone the repository:



git clone https://github.com/<your-username>/InsureClaim-Multi-Embedding-RAG.git

cd InsureClaim-Multi-Embedding-RAG



Install dependencies:



pip install -r requirements.txt

Gemini API Configuration



The application requires a Google Gemini API key.



Set it as an environment variable rather than placing the key directly in the source code.



Windows CMD

set GOOGLE\_API\_KEY=YOUR\_API\_KEY

PowerShell

$env:GOOGLE\_API\_KEY="YOUR\_API\_KEY"



Do not commit API keys, .env files, or other credentials to GitHub.



Running the Application



Start the Streamlit application:



streamlit run app\_multi\_embedding.py



The application will open in your browser.



Upload a policy document and enter a question related to its contents.



The system can then retrieve relevant policy passages and generate an evidence-grounded response.



Example Questions



The application can be used for questions such as:



What conditions are covered under this policy?



What exclusions are mentioned in the policy?



What documents are required to submit a claim?



What is the waiting period?



What are the eligibility requirements?



What are the claim settlement conditions?

Retrieval Workflow



For each query, the system follows:



Question

&#x20;  ↓

Query Embedding

&#x20;  ↓

FAISS Similarity Search

&#x20;  ↓

Relevant Policy Chunks

&#x20;  ↓

Evidence Selection

&#x20;  ↓

Gemini Answer Generation

&#x20;  ↓

Confidence + Sources



When comparing models, the same question is processed independently through each embedding model, allowing their retrieval and answer characteristics to be inspected side by side.



Evaluation Dataset



The project includes a curated gold test dataset containing policy-related questions and reference answers.



This provides a foundation for evaluating:



Retrieval quality

Answer correctness

Evidence relevance

Embedding model differences



The dataset is included as gold\_test\_dataset.json.



Why Multi-Embedding RAG?



Embedding models represent text differently.



A model optimized for general semantic similarity may retrieve different passages from a model designed specifically for question-answering.



By maintaining independent vector stores for multiple embedding models, InsureClaim makes these differences observable rather than hiding them behind a single retrieval configuration.



This provides a practical framework for studying the relationship between:



Embedding Model

&#x20;      ↓

Semantic Retrieval

&#x20;      ↓

Retrieved Evidence

&#x20;      ↓

Generated Answer

&#x20;      ↓

Answer Confidence

Security



API credentials are loaded through the GOOGLE\_API\_KEY environment variable.



Credentials should never be hardcoded into source files or committed to version control.



Future Improvements



Potential extensions include:



Hybrid BM25 + vector retrieval

Reranking with cross-encoder models

Automated retrieval evaluation

Better confidence calibration

Citation-level answer verification

Persistent vector-store management

Support for additional embedding models

Deployment as a cloud-hosted application

