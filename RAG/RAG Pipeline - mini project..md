# **RAG Pipeline — Stages Summary (Extended Stage 1)**

| **Stage**             | **Brief Description (>20–50+ words for Stage 1)**                                                                                                                                                                                                                                                                                                                                                                    |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Ingestion**      | This stage collects and imports source materials that contain knowledge the system will reference. Users can upload PDFs, TXT files, DOCX documents, or large batches of files from folders or cloud storage providers. The ingestion stage ensures all external knowledge is centralized in one structured entry point, enabling scalable indexing and consistent processing regardless of format, size, or origin. |
| **2. Preprocessing**  | Document loaders extract raw text and metadata while applying cleanup steps to remove noise, formatting artifacts, and unnecessary structural elements for reliable processing.                                                                                                                                                                                                                                      |
| **3. Chunking**       | Large text blocks are split into smaller, meaningful segments using structured chunking with token size limits and overlap to ensure contextual continuity and semantic coherence.                                                                                                                                                                                                                                   |
| **4. Embeddings**     | Each chunk is transformed into high-dimensional vector representations using embedding models, enabling mathematical similarity comparison and contextual understanding for retrieval.                                                                                                                                                                                                                               |
| **5. Vector Storage** | The generated embeddings are stored in a persistent vector database that supports efficient indexing, similarity search, and scalable storage for fast real-time retrieval.                                                                                                                                                                                                                                          |
| **6. Retrieval**      | When a user submits a query, the system searches the vector database to return the most relevant chunks using similarity scoring and MMR to maximize information diversity.                                                                                                                                                                                                                                          |
| **7. LLM Reasoning**  | Retrieved context is combined with the user query and passed to the LLM, which generates accurate, grounded answers instead of hallucinating unsupported responses.                                                                                                                                                                                                                                                  |
| **8. Output**         | The system returns a final response enriched with source references to ensure transparency, credibility, and the ability to verify where the information originated.                                                                                                                                                                                                                                                 |

---

Absolutely — here is the **updated and validated** version of:


# **1. Ingestion — Overview**

Ingestion is the first stage of a RAG pipeline where external knowledge sources are collected and imported into the system. It consolidates information stored across multiple formats—PDFs, TXT, DOCX, web pages, spreadsheets, and cloud storage—into a single uniform entry point. This stage validates and reads documents, extracts raw text and metadata, handles encoding or structure issues, and prepares content for subsequent preprocessing, chunking, embedding, and vector storage. A strong ingestion layer ensures consistent and scalable data handling, enabling seamless ingestion from a single file to enterprise-scale repositories.

## **Imports Table (Latest Support — Libraries & Purpose)**

| **Import Name**        | **Purpose / Role**                                        |
| ---------------------- | --------------------------------------------------------- |
| `PyPDFLoader`          | Load a single PDF and extract page-level text + metadata. |
| `PyPDFDirectoryLoader` | Load multiple PDF files recursively from a directory.     |
| `os`                   | File path & directory utilities.                          |

## **Example Code — Single PDF Ingestion (Latest Recommended)**

```python
from langchain_community.document_loaders.pdf import PyPDFLoader

# Path to PDF file
pdf_path = "data/sample.pdf"

# Load PDF
loader = PyPDFLoader(file_path=pdf_path)
documents = loader.load()

# Preview results
print("Total pages:", len(documents))
print("Metadata:", documents[0].metadata)
print("Sample text from first page:\n")
print(documents[0].page_content[:600])  # Preview
```

## **Folder Ingestion Example — Latest Method**

```python
from langchain_community.document_loaders.pdf import PyPDFDirectoryLoader

# Load all PDFs inside the directory (recursive supported)
dir_loader = PyPDFDirectoryLoader(path="data/", glob="**/*.pdf")
all_documents = dir_loader.load()

print("Total documents loaded from directory:", len(all_documents))
```

---
# 2. Preprocessing
Preprocessing is the second stage of a RAG pipeline where raw ingested documents are cleaned, normalized, and prepared for chunking and embedding. This stage removes unnecessary formatting artifacts such as extra whitespace, page numbers, repeated headers/footers, and invisible characters. It may also include text normalization (lowercasing, punctuation handling), language detection, deduplication, splitting large texts, and preserving important metadata. The purpose of preprocessing is to ensure clean, consistent, and semantically meaningful text so the downstream embedding and retrieval stages produce accurate search results and reduce noise. A solid preprocessing step improves retrieval quality, model grounding accuracy, and final answer reliability.

## **Imports Table (Latest Support — Libraries & Purpose)**

|**Import Name**|**Purpose / Role**|
|---|---|
|`RecursiveCharacterTextSplitter`|Splits cleaned text into structured chunks with size + overlap control.|
|`Document`|Defines standardized text+metadata object format after preprocessing.|
|`typing.List`|Type hints for handling document collections.|
## **Example Code — Basic Preprocessing & Text Split**

```python
from langchain_text_splitters import RecursiveCharacterTextSplitter
from langchain_core.documents import Document
from typing import List

# Example: documents loaded from ingestion stage
# 'documents' is a list of Document objects from PyPDFLoader or directory loader

# Create text splitter for preprocessing & chunk preparation
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=200,
    length_function=len
)

# Apply splitting
processed_docs: List[Document] = text_splitter.split_documents(documents)

print("Original document count:", len(documents))
print("Processed chunk count:", len(processed_docs))
print("Sample processed chunk:\n")
print(processed_docs[0].page_content[:400])
```
## **Optional: Cleaning & Normalization Example**

```python
# Example manual preprocessing before split
cleaned_docs = []
for doc in documents:
    text = doc.page_content
    text = text.replace("\n", " ").strip()
    cleaned_docs.append(Document(page_content=text, metadata=doc.metadata))

print("Cleaned document example:\n")
print(cleaned_docs[0].page_content[:400])
```

---
# **3. Chunking — Overview**

Chunking is the third stage of the RAG pipeline where cleaned documents are broken into smaller, semantically meaningful text segments. Because embedding models and vector databases work best with moderate-sized inputs, chunking ensures that each piece of text fits within token limits while preserving contextual continuity. The goal is to produce chunks that are neither too large (causing loss of specificity and high embedding cost) nor too small (causing context fragmentation and poor search accuracy). Chunking typically uses parameters like `chunk_size` and `chunk_overlap` to maintain continuity across boundaries. Proper chunking significantly improves retrieval precision, reduces hallucinations, and increases the relevance of responses generated later by the LLM.

---

## **Imports Table (Latest Support — Libraries & Purpose)**

|**Import Name**|**Purpose / Role**|
|---|---|
|`RecursiveCharacterTextSplitter`|Break documents into structured chunks with overlap & size control.|
|`Document`|Standard document class holding chunk text + metadata.|
|`typing.List`|Handle lists of Document objects.|

---

## **Example Code — Recommended Chunking Implementation**

```python
from langchain_text_splitters import RecursiveCharacterTextSplitter
from langchain_core.documents import Document
from typing import List

# Example: 'processed_docs' comes from Stage 2 preprocessing step

text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,      # target chunk length
    chunk_overlap=200,    # maintains context across chunks
    length_function=len
)

# Perform chunking
chunks: List[Document] = text_splitter.split_documents(processed_docs)

print("Total chunks generated:", len(chunks))
print("Sample chunk preview:\n")
print(chunks[0].page_content[:500])
```

---

## **Chunk Parameter Guidance Example**

```python
recommended_settings = {
    "short_docs": {"chunk_size": 500, "chunk_overlap": 100},
    "medium_docs": {"chunk_size": 1000, "chunk_overlap": 200},
    "long_technical_docs": {"chunk_size": 2000, "chunk_overlap": 250},
}

print("Recommended chunk settings:", recommended_settings)
```

---
Perfect — continuing the pipeline with **Stage 5 — Vector Storage**, following the exact same structure.

---

# **5. Vector Storage — Overview**

Vector Storage is the fifth stage in the RAG pipeline where embeddings generated from text chunks are stored in a vector database. This specialized database enables fast similarity search through high-dimensional vectors using algorithms like Approximate Nearest Neighbor (ANN), cosine similarity, and distance metrics. The vector store keeps both vectors and metadata, allowing retrieval to return not just similar embeddings but also the associated document chunks and their origins. A reliable vector storage layer ensures scalability, low-latency retrieval, persistence across sessions, and support for indexing, filtering, and hybrid search (text + vector). Popular vector databases include FAISS, ChromaDB, Qdrant, Weaviate, Pinecone, and Milvus—each suited to different scale, deployment models, and production requirements.

---

## **Imports Table (Latest Support — Libraries & Purpose)**

|**Import Name**|**Purpose / Role**|
|---|---|
|`Chroma`|Local vector database for development & lightweight production.|
|`FAISS`|In-memory high-performance ANN index commonly used in research/local systems.|
|`Qdrant`|Persistent scalable vector DB with filtering and cloud/private deployment options.|
|`OpenAIEmbeddings`|Embedding model used to generate embeddings for vector storage.|

---

## **Example Code — Store Vectors with Chroma (Recommended Local Use)**

```python
from langchain_community.vectorstores import Chroma
from langchain_openai import OpenAIEmbeddings

embedding_model = OpenAIEmbeddings(model="text-embedding-3-large")

# Persisting vector database locally
vectorstore = Chroma.from_documents(
    documents=chunks,         # chunks from Stage 3–4
    embedding=embedding_model,
    persist_directory="vector_db"
)

print("Vector DB created and persisted successfully!")
```

---

## **Example Code — Store Vectors in FAISS (Fast In-Memory Index)**

```python
from langchain_community.vectorstores import FAISS
from langchain_openai import OpenAIEmbeddings

embedding_model = OpenAIEmbeddings(model="text-embedding-3-large")

faiss_store = FAISS.from_documents(chunks, embedding_model)
print("FAISS vector store created. Total records:", len(faiss_store.index.ntotal))
```

---

## **Example Code — Qdrant Cloud / Local (Production-grade)**

```python
from langchain_community.vectorstores import Qdrant
from langchain_openai import OpenAIEmbeddings

embedding_model = OpenAIEmbeddings(model="text-embedding-3-large")

qdrant = Qdrant.from_documents(
    documents=chunks,
    embedding=embedding_model,
    location="http://localhost:6333",  # Qdrant local instance
    collection_name="rag_docs"
)

print("Qdrant collection created successfully!")
```

---
Absolutely — continuing with **Stage 6 — Retrieval**, following the same structured format.

---

# **6. Retrieval — Overview**

Retrieval is the sixth stage of the RAG pipeline where the system searches the vector database to find the most relevant document chunks related to the user’s query. Using the stored embedding vectors, the retriever performs similarity search (e.g., cosine similarity) to return top-k matching pieces of context. Advanced retrieval strategies such as MMR (Maximal Marginal Relevance), hybrid search (keyword + vector), and metadata filtering enhance diversity and precision. A strong retrieval layer ensures that only the most useful, non-redundant, and contextually aligned information is passed to the LLM, improving grounded answer accuracy and minimizing hallucinations.

---

## **Imports Table (Latest Support — Libraries & Purpose)**

|**Import Name**|**Purpose / Role**|
|---|---|
|`Chroma`|Vector database used to perform similarity search.|
|`FAISS`|In-memory ANN index supporting fast retrieval.|
|`Qdrant`|Production-grade vector DB supporting filtering & hybrid search.|
|`as_retriever`|Converts vector store to a retriever interface for querying.|
|`MMR` _(config via retriever kwargs)_|Improves result diversity and avoids repetitive context.|

---

## **Example Code — Basic Similarity Search Retrieval (Chroma)**

```python
from langchain_community.vectorstores import Chroma

# Load existing vector store (persisted in Stage 5)
vectorstore = Chroma(persist_directory="vector_db")

# Convert to retriever
retriever = vectorstore.as_retriever(search_kwargs={"k": 5})

# Perform query
results = retriever.invoke("What is quantum entanglement?")
for r in results:
    print(r.page_content[:300], "\n---")
```

---

## **Example Code — MMR Retrieval (More Diversity)**

```python
retriever = vectorstore.as_retriever(
    search_type="mmr",
    search_kwargs={"k": 8, "fetch_k": 20}
)

results = retriever.invoke("Explain the purpose of chunk overlap.")
print("Returned chunks:", len(results))
```

---

## **Example Code — Filtering / Metadata Based Retrieval**

```python
retriever = vectorstore.as_retriever(
    search_kwargs={
        "k": 5,
        "filter": {"source": "chapter_01"}  # Example metadata filter
    }
)

results = retriever.invoke("What are the main principles discussed?")
print(results[0].metadata)
```

Great — continuing with **Stage 7 — LLM Reasoning / Generation**, using the exact structure you established.

---

# **7. LLM Reasoning — Overview**

LLM Reasoning is the seventh stage of the RAG pipeline where retrieved context and the user query are combined to generate a final grounded response. Instead of relying solely on the model’s internal knowledge, the LLM uses retrieved document chunks to reason, synthesize, summarize, and produce accurate answers. This reduces hallucinations and increases traceability since all responses are supported by real source information. Prompt engineering techniques are used to format context, enforce instructions, limit scope, and structure outputs (e.g., QA, summaries, structured formats). Modern RAG pipelines use chain-based execution, where the LLM is invoked through a prompt template, often combined with retrieval to create a Retrieval-Augmented Generation chain (RAG Chain).

---

## **Imports Table (Latest Support — Libraries & Purpose)**

|**Import Name**|**Purpose / Role**|
|---|---|
|`ChatOpenAI`|LLM model interface for reasoning and response generation.|
|`ChatPromptTemplate`|Creates structured templated prompts for controlled responses.|
|`StrOutputParser`|Parses raw model output into usable string format.|
|`RunnablePassthrough`|Passes data through chain stages without modification.|
|`Runnable` chaining|Used to build retrieval → LLM execution pipelines.|

## **Example Code — Basic RAG Chain (Retrieval + LLM)**

```python
from langchain_openai import ChatOpenAI
from langchain.prompts import ChatPromptTemplate
from langchain.schema.output_parser import StrOutputParser
from langchain.schema.runnable import RunnablePassthrough

# LLM setup
llm = ChatOpenAI(
    model="gpt-4o-mini",   # or "gpt-4o", "o3-mini"
    temperature=0.2
)

# Prompt architecture
prompt = ChatPromptTemplate.from_template("""
You are an expert assistant. Use the context below to answer strictly based on retrieved information.

Context:
{context}

Question:
{question}

Provide a concise and accurate answer.
""")

# Create chain
rag_chain = (
    {"context": retriever, "question": RunnablePassthrough()}
    | prompt
    | llm
    | StrOutputParser()
)

# Execute query
response = rag_chain.invoke("What is chunk overlap and why is it needed in RAG?")
print(response)
```

## **Example — Response with Source Reference Formatting**

```python
prompt = ChatPromptTemplate.from_template("""
Answer using only the provided context.
If the answer is not present, say 'I don't know'.

Context:
{context}

Question:
{question}

Return answer + bullet point citations of where context was found.
""")
```

# **8. Output — Overview**

Output is the final stage of the RAG pipeline where the system delivers a structured and meaningful response back to the user. This stage formats the LLM-generated answer, incorporates citations or references to source documents, and applies any additional post-processing such as summary formatting, markdown structuring, answer validation, or UI packaging. A well-designed output layer improves clarity, trust, and usability by showing where information originated, supporting follow-up questions, and enabling integration with applications such as chat interfaces, dashboards, and document analyzers. Output can include text responses, structured JSON, formatted tables, citations, or confidence scores depending on application needs.
## **Imports Table (Latest Support — Libraries & Purpose)**

|**Import Name**|**Purpose / Role**|
|---|---|
|`StrOutputParser`|Extracts clean final answer text from the LLM chain pipeline.|
|`RunnableMap` / `RunnableSequence`|Allows building structured output maps for formatted responses.|
|`pydantic` _(optional)_|Defines structured output models (JSON objects, fields, validation).|
|`json` _(optional)_|Format final answers for APIs or UI integration.|
## **Example Code — Clean Answer Output with Parsed String**

```python
from langchain.schema.output_parser import StrOutputParser

parser = StrOutputParser()

response = parser.invoke("Sample generated answer from the LLM")
print("Final Parsed Output:\n")
print(response)
```
## **Example Code — Output with Citations / Sources**

```python
from langchain.schema.runnable import RunnableParallel

# Combine retrieved context & LLM output for structured result
rag_with_sources = RunnableParallel(
    answer=rag_chain,             # from Stage 7
    sources=retriever             # returns documents that were retrieved
)

result = rag_with_sources.invoke("Explain chunk overlap")
print("Answer:\n", result["answer"])
print("\nSources used:\n")
for source in result["sources"]:
    print(f"- {source.metadata} | preview: {source.page_content[:200]}")
```
## **Example — JSON Output for API Consumers**

```python
import json

json_response = json.dumps({
    "answer": result["answer"],
    "sources": [doc.metadata for doc in result["sources"]],
}, indent=2)

print(json_response)
```

---
