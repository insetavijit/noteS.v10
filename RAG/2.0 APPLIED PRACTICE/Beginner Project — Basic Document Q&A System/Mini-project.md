# 🚀 Modern RAG Q&A System Stack (2025 Research-Updated)

**Project**: 2.2.1 Beginner Document Q&A System **Last Updated**: November 2025

---
## 📊 Research-Backed Technology Stack

| Layer                | Recommended Tool (2025)                                  | Why                                                        |
| -------------------- | -------------------------------------------------------- | ---------------------------------------------------------- |
| **Language Model**   | **`gpt-4o-mini`**                                        | Latest, cost-effective ($0.15/1M tokens), excellent for QA |
| **Embedding Model**  | **`intfloat/e5-base-v2`** or **`BAAI/bge-base-en-v1.5`** | Open-source, high MTEB scores, proven RAG performance      |
| **Vector Store**     | **ChromaDB** or **Qdrant**                               | ChromaDB: simplest start; Qdrant: better filtering & speed |
| **Framework**        | **LlamaIndex** (for beginners)                           | Purpose-built for RAG, simpler API than LangChain          |
| **Document Loaders** | **PyPDFLoader** / **UnstructuredLoader**                 | Robust text extraction, handles complex layouts            |
| **Package Manager**  | **uv** (instead of pip)                                  | 10-100x faster than pip, Rust-based, project management    |
| **UI (optional)**    | **Streamlit** or **CLI**                                 | Rapid prototyping, beginner-friendly                       |

---
## 🎯 Key Research Findings (Nov 2025)

### Embedding Models

Recent benchmarks show that open-source models like E5 and BGE achieve over 85% retrieval accuracy, with E5-base-v2 particularly strong for technical documentation and BGE-base-en-v1.5 reliable across diverse fields including legal and scientific content.

**Why E5/BGE over bge-small?**

- E5-large-v2 is designed for efficient embedding generation and suitable for various NLP tasks, with models supporting up to 512 tokens being sufficient for most cases
- Better balance of speed, accuracy, and dimension size (768 vs 384)
- Proven in production RAG systems

### Vector Databases

ChromaDB is favored by developers for lightweight AI apps and prototypes due to its simplicity and developer-friendly design, while offering the lowest latency crucial for real-time applications.

**When to use alternatives:**

- Qdrant is optimized for real-time, low-latency search capabilities, making it suitable for applications that prioritize fast search performance
- Milvus is engineered for large-scale, distributed environments with elastic and horizontal scalability, excellent for high-performance applications requiring seamless scaling

### Chunking Strategy (Critical!)

Research shows page-level chunking provides the most consistent performance across diverse document types, while factoid queries work well with 256-512 tokens and complex analytical queries benefit from larger 1,024 token chunks.

**Best Practices:**

- Industry best practices recommend 10-20% overlap as a starting point, with 50-100 tokens of overlap for a 500-token chunk
- Use RecursiveCharacterTextSplitter (respects natural boundaries)
- Start with: 512 tokens per chunk, 75 tokens overlap (15%)

### Framework Choice

LlamaIndex shines for straightforward RAG applications with lighter development lift and efficient indexing, while LangChain excels in chaining multiple tasks and building complex workflows.

**For beginners:** LlamaIndex is simpler for document-focused RAG.

### Package Manager

UV is 10-100 times faster than traditional package managers, is written in Rust with a minimal resource footprint, and integrates seamlessly with existing Python packaging standards.

---
## 🏗️ Updated Architecture

```
Documents (PDF/TXT/DOCX)
    ↓
Document Loader + Preprocessing
    ↓
Chunking (RecursiveCharacterTextSplitter)
├─ Size: 512 tokens
├─ Overlap: 75 tokens (15%)
└─ Respects: paragraphs > sentences > words
    ↓
Embeddings (e5-base-v2 or bge-base-en-v1.5)
    ↓
Vector DB (ChromaDB with persistence)
    ↓
Query → Retrieval (top-k with MMR)
    ↓
Context + Query → LLM (gpt-4o-mini)
    ↓
Answer + Sources
```

---
## 📦 Modern Project Structure

```
rag_qa/
├── data/
│   ├── documents/          # Raw documents
│   └── chroma_db/          # Persistent vector store
├── src/
│   ├── __init__.py
│   ├── config.py           # Configuration & settings
│   ├── loaders.py          # Document loading
│   ├── chunking.py         # Chunking strategies
│   ├── embeddings.py       # Embedding generation
│   ├── vectorstore.py      # Vector DB operations
│   ├── retrieval.py        # Query & retrieval logic
│   ├── qa_chain.py         # QA pipeline
│   └── app.py              # Main application / CLI
├── tests/
│   └── test_chunking.py    # Unit tests
├── notebooks/
│   └── experiments.ipynb   # Experimentation
├── .env.example
├── pyproject.toml          # uv project config
├── uv.lock                 # Dependency lock file
└── README.md
```

---

## ⚙️ Installation (Using UV)

```bash
# Install uv (one-time, cross-platform)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Create new project
uv init rag_qa
cd rag_qa

# Add dependencies (LlamaIndex approach - simpler for beginners)
uv add llama-index
uv add llama-index-embeddings-huggingface
uv add llama-index-vector-stores-chroma
uv add chromadb
uv add python-dotenv
uv add pypdf

# Optional: Add UI
uv add streamlit

# Optional: Add testing
uv add pytest --dev
```

**Alternative with LangChain (if you prefer):**

```bash
uv add langchain langchain-community langchain-openai
uv add langchain-huggingface
uv add chromadb sentence-transformers
uv add pypdf python-dotenv
```

---
## 🎯 Implementation Plan (Research-Optimized)

### **Phase 1: Core RAG Pipeline (Days 1-2)**

**Step 1.1 - Document Loading**

- Use PyPDFLoader for PDFs
- Add error handling for corrupted files
- Extract metadata (filename, page numbers)

**Step 1.2 - Smart Chunking** ⭐ Most Important

- RecursiveCharacterTextSplitter configuration:
    
    ```python
    chunk_size=512         # tokens, not characterschunk_overlap=75       # 15% overlapseparators=["\n\n", "\n", ". ", " ", ""]
    ```
    
- Test with your actual documents
- Monitor chunk quality (avoid mid-sentence breaks)

**Step 1.3 - Embeddings**

- Load `intfloat/e5-base-v2` via HuggingFace
- Batch processing for efficiency
- Cache embeddings to disk

**Step 1.4 - Vector Store**

- ChromaDB with persistence
- Test similarity search manually
- Add metadata filtering capability

### **Phase 2: QA System (Days 3-4)**

**Step 2.1 - Retrieval Strategy**

- Use MMR (Maximum Marginal Relevance) instead of pure similarity
- Retrieve top-5 chunks by default
- Add re-ranking for better results (optional)

**Step 2.2 - Prompt Engineering**

```
System: You are a helpful AI assistant. Answer based ONLY on the context.
If unsure, say "I don't have enough information."

Context: {retrieved_chunks}

Question: {user_question}

Answer:
```

**Step 2.3 - Response Generation**

- Use gpt-4o-mini with temperature=0 (factual)
- Include source citations
- Add confidence scores

### **Phase 3: Evaluation & Refinement (Day 5)**

**Step 3.1 - Create Test Set**

- 10-20 representative questions
- Known correct answers
- Edge cases (ambiguous, out-of-scope)

**Step 3.2 - Measure Performance**

- Retrieval accuracy (are correct chunks retrieved?)
- Answer quality (manual review)
- Latency (< 3 seconds end-to-end)

**Step 3.3 - Iterate**

- Adjust chunk size based on results
- Tune retrieval parameters (top-k, MMR)
- Refine prompts

---
## 🚀 Quick Start Example (LlamaIndex)

```python
# main.py
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader
from llama_index.embeddings.huggingface import HuggingFaceEmbedding
from llama_index.llms.openai import OpenAI
from llama_index.core.node_parser import SentenceSplitter

# Load documents
documents = SimpleDirectoryReader("data/documents").load_data()

# Configure chunking (research-optimized)
splitter = SentenceSplitter(
    chunk_size=512,
    chunk_overlap=75,
)

# Configure embedding
embed_model = HuggingFaceEmbedding(
    model_name="intfloat/e5-base-v2"
)

# Configure LLM
llm = OpenAI(model="gpt-4o-mini", temperature=0)

# Build index
index = VectorStoreIndex.from_documents(
    documents,
    transformations=[splitter],
    embed_model=embed_model,
)

# Query
query_engine = index.as_query_engine(
    llm=llm,
    similarity_top_k=5,
)

response = query_engine.query("What is supervised learning?")
print(response)
```

---
## 📈 What You'll Learn (Enhanced)

|Skill|Why It Matters|2025 Context|
|---|---|---|
|**RAG fundamentals**|Core competency for AI apps|90% of production LLM apps use RAG|
|**Chunking optimization**|Chunking choice determines retrieval quality, with up to 9% gap in recall performance between strategies|Single biggest performance lever|
|**Embeddings & vector search**|Foundation for semantic retrieval|Open models now rival proprietary ones|
|**Evaluation methodology**|Measure real improvements|Production requires metrics, not intuition|
|**Modern Python tooling**|Professional development practices|UV is industry trend (10-100x speedup)|

---
## 🎁 Expected Output

```
Question: What is supervised learning?

Answer:
Supervised learning is a machine learning paradigm where models learn
from labeled training data to predict outcomes for new, unseen data.
The algorithm learns the mapping between input features and target
labels by minimizing prediction errors during training. Common
algorithms include linear regression, decision trees, support vector
machines, and neural networks.

Key characteristics:
- Requires labeled training data (input-output pairs)
- Learns predictive relationships
- Evaluated on test set performance

Sources:
[1] introduction_to_ml.pdf, page 3, chunk 2
    "Supervised learning uses labeled examples..."
[2] ml_algorithms.pdf, page 12, chunk 5
    "Common supervised learning algorithms include..."

Confidence: High (retrieved 5/5 relevant chunks)
Response time: 2.3 seconds
```

---
