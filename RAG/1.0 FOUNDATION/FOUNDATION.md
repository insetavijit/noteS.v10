
> [!quote] **Hitopadesha** (Niti Shastra, India, ~12th Century)  
> **“उद्योगिनं पुरुषसिंहमुपैति लक्ष्मीः।”**  
> **Pronunciation:** _Udyoginam puruṣa-siṃham upaiti lakṣmīḥ_  
> _“Prosperity approaches the one who strives like a lion.”_

# **RAG FOUNDATIONS : A Ocean's Snapshot**

> The purpose of this Foundations module is to build a strong conceptual baseline before writing code or constructing systems. Think of this section as the high-altitude map. We will explore deep internals and production workflows inside the Applied Practice module.

---

## **[[1.1 Definitions – brief overview of essential terms and concepts]]**

- **RAG (Retrieval-Augmented Generation)** – combining external knowledge retrieval with LLM reasoning
    
- **Retrieval** – searching a knowledge base for relevant text to answer a query
    
- **Embeddings** – mathematical vector representation of text that captures semantic similarity
    
- **Vector Database / Vector Store** – database optimized for similarity search over embeddings (FAISS / Milvus / Pinecone / Qdrant)
    
- **Chunking** – splitting long documents into smaller units for accurate retrieval
    
- **Similarity Search** – finding top-k closest vectors using cosine / dot product / Euclidean distance
    
- **Reranking** – re-scoring candidate results to refine relevance
    
- **Context Window** – maximum token capacity the LLM can process at once
    
- **Grounding** – injecting real factual information to minimize hallucination
    
- **Hallucination** – incorrect output confidently produced by the model without real evidence
    

---

## **[[1.2 Core Principles – guiding rules, fundamentals, and theoretical base]]**

- **Externalized knowledge** – LLM does reasoning, vector DB holds facts
    
- **Retrieve, then generate** – search first, answer after
    
- **Precision over quantity** – fewer relevant chunks > many noisy chunks
    
- **Embeddings quality determines accuracy** – better vector representation → higher relevance
    
- **Chunking strategy matters** – structured segmentation improves search quality
    
- **Reranking reduces nonsense** – filters out retrieved but irrelevant text
    
- **Optimize system-wide, not piecewise** – retrieval + prompting + generation = single pipeline
    
- **Observability is critical** – track retrieval hit-rate, latency, hallucination rate
    

---

## **[[1.3 Mental Models – intuitive ways to understand how the system works]]**

- **“LLM = Thinker, Vector DB = Memory”** – reasoning separate from stored knowledge
    
- **“RAG is Search + Summarize”** – retrieve docs → synthesize answer
    
- **“Chunk = Page, Document = Book”** – small pages are easier to search
    
- **“Reranker = Editor”** – decides what information actually matters
    
- **“Prompt = Stage Setup”** – retrieved context sets the scene for the LLM
    

---

## **[[1.4 Architecture Overview – structural components and their interactions]]**

### **1.4.1 High-Level Diagram – visual summary of the system**

```
User Query
      ↓
Query Embeddings
      ↓
Similarity Search → Vector DB
      ↓
Top-K Relevant Chunks → Reranker (optional)
      ↓
Context-Injected Prompt → LLM
      ↓
Generated Answer
```

### **[[1.4.2 Components & Responsibilities – what each part does]]**

- **Ingestion Pipeline** – clean, segment, embed and index documents
    
- **Embedding Model** – converts text to vectors capturing semantic meaning
    
- **Vector Store** – efficient nearest-neighbor search of embeddings
    
- **Retriever** – fetches the best matching chunks
    
- **Reranker** – improves precision by deeper scoring
    
- **Prompt Builder** – constructs final LLM prompt with context
    
- **LLM Generator** – synthesizes answer using retrieved knowledge
    
- **Telemetry / Monitoring** – inspect pipeline quality & failures
    

### **[[1.4.3 Data Flow – how information moves through the system]]**

- **Raw Document** → clean → **chunk** → embed → **store**
    
- **User query** → embed → **retrieve top-k** → rerank → **build prompt** → LLM output
    
- **Optional feedback loop** → caching & refinement
    

---

## **[[1.5 Internals & Mechanics – behind-the-scenes processes and algorithms]]**

- **Vector embedding encoding** – text → numeric vector (e.g. 768–3072 dimensions)
    
- **ANN (Approximate Nearest Neighbor) search** – HNSW / IVF-PQ / Flat
    
- **Similarity metrics** – cosine / dot product / L2 distance
    
- **Cross-encoder reranking** – pairwise scoring for accuracy
    
- **Structured prompt injection** – sectioned context, citation formatting
    
- **Context compression** – summarization before LLM input
    
- **Caching & hybrid search** – speed optimization for production scale
    

---

## **[[1.6 Limitations & Trade-offs – what it cannot do and constraints to consider]]**

- **Context window limits** restrict how much data can be inserted
    
- **Latency increases** as data size & search complexity grow
    
- **Poor chunking leads to irrelevant retrieval**
    
- **Bad embeddings → incorrect context selection**
    
- **Hallucinations still possible without tight grounding**
    
- **Reranking increases accuracy but adds cost & time**
    
- **Scalability challenges** with massive datasets
    

---

---

## **Choose what to generate next:**

a) **Basic RAG Code Example** (end-to-end minimal)  
b) **Detailed Architecture Diagram** (clean + labelled)  
c) **Cheatsheet for chunking & embeddings**  
d) **Industry-grade patterns & workflows**  
e) **Flashcards for Section 1**

**Reply with: a / b / c / d / e / all** 🚀