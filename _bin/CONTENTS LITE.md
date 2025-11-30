## 1. FOUNDATIONS — Ocean’s Snapshot

### 1.1 Definitions — Essential Terms

```python
│   ├── LLM — Large Language Model generating natural language output
│   ├── RAG — Retrieval-Augmented Generation combining search + generation
│   ├── Corpus — Source documents used for retrieval
│   ├── Embeddings — Vector representation of text meaning
│   ├── Vector Store — Storage for dense embeddings enabling semantic search
│   ├── Retriever — Component to fetch relevant chunks based on similarity
│   ├── Chunking — Splitting large text into manageable sections
│   ├── Prompt Template — Structured instruction for the LLM
│   ├── Chain — Execution pipeline in LangChain
│   ├── Document Loader — Load raw files into text form
│   └── Metadata — Document attributes for filtering and ranking
```

### 1.2 Core Principles — Fundamentals

```python
│   ├── Retrieval before Generation — Query → retrieve → augment → answer
│   ├── Relevance over Recall — Precision > Quantity of documents retrieved
│   ├── Context Window Efficiency — Fit chunks within token limits
│   ├── Semantic Search — Vector similarity over keyword searching
│   ├── Deterministic Prompting — Repeatable responses through structure
│   ├── Source Grounding — Evidence-backed responses
│   └── Iterative Improvement — Evaluate → refine → repeat
```

### 1.3 Mental Models — Intuitive Understanding

```python
│   ├── RAG as Brain + Library — LLM thinks, retriever remembers
│   ├── Embeddings as Coordinates — Meaning placed in high-dimensional space
│   ├── Chunking as Puzzle Pieces — Smaller parts fit into limited context
│   ├── Prompt as Law — LLM follows enforced structure
│   ├── Vector DB as Google Search — Results sorted by semantic closeness
│   └── Retrieval as Evidence — Answers built from actual data, not guessing
```

### 1.4 Architecture Overview — System Structure

```python
│   ├── Input → Embed → Store → Retrieve → Rerank → LLM → Output
│   ├── Components — Loader →Splitter →Embeddings →VectorDB →Retriever →Chain
│   └── Deployment Flow — Offline ingestion → Online querying
```

### 1.5 Internals & Mechanics — Under the Hood

```python
│   ├── Similarity Search — cosine / dot product / euclidean
│   ├── Embedding Model — text encoder converting tokens to dense vectors
│   ├── Vector Indexing — IVF, HNSW, Flat, PQ structures
│   ├── Ranking — Score ordering by semantic distance
│   ├── Document Relevance — filtering, reranking, hybrid search
│   ├── Context Assembly — Final prompt packaging + formatting
│   └── Generation Step — LLM consumes retrieved knowledge
```

### 1.6 Limitations & Trade-offs

```python
│   ├── Retrieval Quality Dependency — Better chunks = better results
│   ├── No Reasoning Without Evidence — Weak corpus → weak answers
│   ├── Context Window Limits — Truncation risk
│   ├── Embedding Cost — Expensive for large datasets
│   ├── Update Complexity — Rebuild indexes when data changes
│   └── Latency — Retrieval + generation adds overhead
```

---

## 2. CODE EXAMPLES — Growing Difficulty

### 2.1 Code Examples — Progression

#### 2.1.1 Basic RAG Pipeline

```python
│   ├── Document loading — DirectoryLoader / PyPDFLoader / WebLoader
│   ├── Chunking — RecursiveCharacterTextSplitter
│   ├── Embeddings — OpenAI / HuggingFace / Ollama / Cohere
│   ├── VectorStore — FAISS / Chroma / Pinecone / Milvus / Weaviate
│   └── RetrievalQA Chain — Querying with RAG
```

#### 2.1.2 Intermediate Pipeline Enhancements

```python
│   ├── Advanced Prompt Templates — structure, tone, rules
│   ├── Reranking — multi-step search + score filtering
│   ├── Metadata Filtering — file type, tags, category fields
│   ├── Hybrid Retrieval — vector + keyword + rerank combination
│   └── Conversational RAG — Memory + context tracking
```

#### 2.1.3 Advanced Features & Optimizations

```python
│   ├── Multi-vector Index — Separate summaries, keywords, content vectors
│   ├── Query Rewriting — Transform user query to improved search query
│   ├── Agent RAG — Tools + planning + reasoning
│   ├── Streaming Output — token streaming
│   └── Evaluation — hallucination / grounding / precision scoring
```

---

### 2.2 Hands-on Mini Projects

```python
│   ├── Chat with PDF / Docs — multi-file ingestion + retrievalQ&A
│   ├── Knowledge Base Assistant — custom domain RAG
│   └── RAG API — Query endpoint via FastAPI / Flask
```

---

### 2.3 Patterns & Workflows

```python
│   ├── Ingestion Pipeline — offline build and persist embeddings
│   ├── Query Pipeline — real-time retrieval + generation
│   ├── Modular Pipeline — loaders → splitters → embeddings → retrievers
│   └── Evaluation Loop — score, correct, refine
```

---

### 2.4 Tools, Tips & Debugging

```python
│   ├── LangSmith — tracing, visualization, evaluation
│   ├── LLM-eval — benchmark pipeline performance
│   ├── Prompt Playground — experiment formats and templates
│   └── Logging & Instrumentation — timing, memory, accuracy
```

---

### 2.5 Real-World Use Cases

```python
│   ├── Enterprise Search
│   ├── Documentation Assistants
│   ├── Customer Support Bots
│   ├── Legal / Medical Evidence Retrieval
│   ├── Financial Research / Trading Intelligence
│   └── Data-heavy analytics assistants
```

---

## 3. QUICK REFERENCE

### 3.1 Cheatsheets — One-page summaries

```python
│   ├── RAG Pipeline Cheatsheet — ingest, embed, store, search, generate
│   ├── Vector DB Comparison — Chroma, FAISS, Pinecone, Milvus, Weaviate
│   ├── Chunking Strategies — size, overlap, hierarchical splitting
│   ├── Retrieval Strategies — top-k, hybrid, rerank
│   ├── Prompt Templates — answer, summary, expand, cite
│   └── Evaluation Metrics — precision, recall, grounding score
```

### 3.2 Snippets — Copy-Paste Ready

```python
│   ├── RAG Basic Pipeline Code
│   ├── Conversational Retrieval Chain
│   ├── Hybrid Search example
│   ├── FastAPI Query Endpoint
│   └── Persist / Load existing vector DB
```

### 3.3 Templates — Reusable Structures

```python
│   ├── Project Folder Skeleton
│   ├── Prompt Template Boilerplate
│   ├── Evaluation Script Template
│   └── Vector DB persistence template
```

### 3.4 Architecture Diagrams

```python
│   ├── ingestion → embeddings → vectorDB build
│   ├── query → retriever → rerank → LLM → response
│   └── agents + tools + RAG flow
```

### 3.5 Error & Issue Playbook

```python
│   ├── Poor search results — fix chunking, indexing, rerank
│   ├── Hallucinations — enforce grounding & structured prompts
│   ├── High latency — reduce k, enable persistence, choose fast embeddings
│   └── Memory leaks — cleanup, batch, chunk, delete
```

### 3.6 Best Practices

```python
│   ├── Always retrieve before LLM
│   ├── Use structured prompts
│   ├── Track performance with LangSmith
│   ├── Persist embeddings
│   └── Evaluate regularly
```

---

## 4. ACTIVE RECALL

### Core Principle

```python
│   ├── Retrieval beats memorization
│   ├── Context grounded answers prevent hallucination
│   └── Data quality dictates output quality
```

### 4.1 Flashcards (Q/A)

```python
│   ├── What is RAG?
│   ├── What is a vector store?
│   ├── Why chunk documents?
│   └── What is semantic search?
```

### 4.2 Quizzes

```python
│   ├── Beginner — define components
│   ├── Intermediate — build a basic pipeline
│   └── Advanced — optimize retrieval ranking
```

### 4.3 Challenges

```python
│   ├── Build RAG from scratch without docs
│   ├── Add reranking and measure accuracy
│   └── Convert into API + UI
```

### 4.4 Memory Anchors

```python
│   ├── RAG = Library + Brain
│   ├── Embeddings = Coordinates
│   └── Vector DB = Semantic index
```

### 4.5 Spaced Repetition Plan

```python
│   ├── 1d — pipeline rewrite from memory
│   ├── 7d — add reranking
│   ├── 30d — deploy to production
│   └── 90d — evaluate and improve
```

---
