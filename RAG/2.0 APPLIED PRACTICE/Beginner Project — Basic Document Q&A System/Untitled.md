# 2.2.1 Beginner Project — Basic Document Q&A System

## Goal

Build a complete Retrieval-Augmented Generation (RAG) workflow that can upload documents (PDF/TXT), convert to text, split into chunks, embed, store in a vector database, and perform intelligent question answering.

**Estimated Time:** 1-2 hours  
**Difficulty:** Beginner-friendly
## A. Core Workflow — Pipeline Breakdown

| Component           | Description                                     | Tools / Options                                            |
| ------------------- | ----------------------------------------------- | ---------------------------------------------------------- |
| **Ingest Document** | Upload & extract raw text from files            | `pypdf`, `pdfplumber`, native `.txt`                       |
| **Chunk Text**      | Split into blocks for embedding                 | LangChain `RecursiveCharacterTextSplitter`                 |
| **Embed**           | Convert chunks → vector embeddings              | `OpenAIEmbeddings` (paid) / `HuggingFaceEmbeddings` (free) |
| **Store**           | Vector database for retrieval                   | `FAISS` (recommended for beginners)                        |
| **Retrieve**        | Fetch semantically similar chunks               | `SimilaritySearch` with `top_k=3-5`                        |
| **Answer**          | Generate final response using retrieved context | LangChain `RetrievalQA`                                    |

---

## B. Expected Behavior Examples

### ✅ Successful Query

**Input:** "What is the key idea in chapter 1?"

**System Process:**

1. Embeds the question
2. Retrieves 3-5 most relevant chunks from vector DB
3. Passes chunks + question to LLM

**Output:** "The key idea in chapter 1 is that retrieval-augmented generation combines the factual grounding of search with the fluency of language models..."

---

### ⚠️ Handling Missing Information

**Input:** "What is the author's opinion on quantum computing?"

**System Process:**

1. Retrieves chunks but finds no relevant matches
2. LLM recognizes insufficient context

**Output:** "I don't have information about quantum computing in the provided documents. The available content focuses on machine learning and NLP."

---

## C. Common Pitfalls & Solutions

|Problem|Cause|Solution|
|---|---|---|
|Empty or garbled text extraction|PDF has images/scanned content|Use OCR tools like `pytesseract` or provide text-based PDFs|
|"Rate limit exceeded" error|Too many API calls to OpenAI|Add delays between calls or switch to local embeddings|
|Irrelevant answers|Chunks too large or small|Adjust chunk size: try 500-1000 chars with 10-20% overlap|
|"Context length exceeded"|Too many chunks retrieved|Reduce `top_k` from 5 to 3, or use smaller chunks|

---

## D. Quick Visual Flow

```
📄 Document Upload
      ↓
📝 Extract Text (pypdf/pdfplumber)
      ↓
✂️ Split into Chunks (RecursiveCharacterTextSplitter)
      ↓
🔢 Generate Embeddings (OpenAI/HuggingFace)
      ↓
💾 Store in Vector DB (FAISS)

                    [System Ready]

❓ User Question
      ↓
🔢 Embed Question
      ↓
🔍 Retrieve Similar Chunks (top_k=3-5)
      ↓
🤖 LLM Synthesis (RetrievalQA)
      ↓
💬 Final Answer
```

---
