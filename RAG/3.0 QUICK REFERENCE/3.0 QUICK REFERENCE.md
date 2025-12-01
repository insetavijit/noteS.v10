> [!quote] **महाभारतम्** (Epic Literature, India, ~400 BCE)  
> **“विद्या विवादाय धनं मदाय शक्तिः परेषां परिपीडनाय।”**  
> _“Knowledge used for arguments, wealth used for pride, and power used to crush others are all misused.”_

## **3. QUICK REFERENCE (RAG – XX Sheets)**

High-speed access to essential knowledge — built for instant utility and rapid implementation.  
Lookup → Decide → Apply → Deploy — without deep-dive reading.

RAG Roadmap#3.1 Cheatsheets – condensed one-page semantic summaries | RAG Roadmap#3.2 Snippets – reusable solution patterns | RAG Roadmap#3.3 Templates – prompt & system frameworks | RAG Roadmap#3.4 Condensed Architecture Diagrams | RAG Roadmap#3.5 Error & Issue Playbook | RAG Roadmap#3.6 Best Practices – recommended configurations  
Back to Top

---

### **3.1 Cheatsheets – condensed one-page summaries**

Fast-lookup documents summarizing essential terminology, processes, and workflow sequences.

- Retrieval Pipeline Overview
    
- Embedding Models & dimensionality reference sheet
    
- Chunking strategies & optimal sizes per content type
    
- Similarity metrics comparison (cosine / dot / Euclidean)
    
- Vector DB capabilities comparison chart (FAISS / Milvus / Pinecone / Qdrant)
    
- Reranking models & scoring heuristics
    
- Prompt patterns for context-aware LLM responses
    
- Context window optimization playbook
    

---

### **3.2 Snippets – copy-paste ready solution patterns**

Small reusable structures solving repetitive design tasks (no code, just pattern formats).

- Query rewrite → retrieve → refine → answer loop
    
- Multi-retriever fusion (keyword + semantic + metadata)
    
- Hybrid scoring formula for ranking candidates
    
- Metadata filtering structure for domain precision
    
- Prompt assembling structure (system + retrieved chunks + query)
    
- Cache strategy outline for fast repeated lookups
    
- Retrieval monitoring pipeline (hit-rate, latency, chunk sources)
    

---

### **3.3 Templates – reusable structures for faster work**

#### **3.3.1 Prompt Templates – structured instruction patterns**

- Context-aware QA template with citations
    
- Multi-document summary synthesis prompt
    
- Verification guardrail prompt (fact-checking)
    
- Hallucination-reduction reasoning chain scaffold
    

#### **3.3.2 System Templates – reusable outlines**

- RAG pipeline architecture template (ingestion, retrieval, prompt, generation)
    
- Retrieval-first routing template (query rewrite → search → rerank)
    
- Production prompt construction framework (role, objective, context injection)
    
- Telemetry + feedback loop structure
    

#### **3.3.3 Boilerplates – starter project layouts**

- Minimal RAG document QA system structure
    
- Production RAG API layout (retrieval + rerank + answer generator)
    
- Multi-tenant enterprise RAG design blueprint
    
- Analytics & monitoring instrumentation layout
    

---

### **3.4 Condensed Architecture Diagrams – minimal visual summaries**

Mini-maps suitable for Obsidian / Notion / Mermaid-ready.

- RAG Pipeline Overview — Query → Retrieve → Rerank → Generate
    
- Ingestion pipeline — Raw docs → Clean → Chunk → Embed → Store
    
- Context-injection flow — Retrieval → pre-prompt → system prompt → answer
    
- Offline vs Online retrieval architectures
    
- Hybrid retrieval model: vector search + keyword search fusion
    

---

### **3.5 Error & Issue Playbook – common problems and fixes**

#### **3.5.1 Common Errors**

- Irrelevant retrieval (low quality top-k)
    
- Hallucinated or factually incorrect outputs
    
- Repetitive or vague responses
    
- Latency spikes in large datasets
    
- Context overflow or truncation
    
- Retrieval misses due to bad chunking
    

#### **3.5.2 Typical Causes**

- Weak or inappropriate embedding model
    
- Chunk size too small or too large
    
- No reranking applied
    
- Missing metadata filters
    
- Vector index not optimized
    
- Poor prompt structure
    

#### **3.5.3 Rapid Fix Strategies**

- Test multiple chunk sizes
    
- Switch to domain-specific embeddings
    
- Add reranking or hybrid search
    
- Prompt restructuring & context ordering
    
- Implement caching + pre-computed responses
    
- Replace noisy documents or clean formatting
    

---

### **3.6 Best Practices – recommended techniques**

#### **3.6.1 Do’s & Don’ts**

- **Do:** monitor retrieval hit-rate, document coverage, latency
    
- **Do:** track which chunks influenced answers
    
- **Do:** enforce structured prompting & citations
    
- **Don’t:** rely only on embeddings without reranking
    
- **Don’t:** inject excessive context into prompts
    
- **Don’t:** ignore data cleanliness
    

#### **3.6.2 Performance Guidelines**

- Use hybrid search for ambiguous queries
    
- Cache embeddings and retrieval results
    
- Async & batch embeddings for ingestion
    
- Use ANN indexes for large datasets
    
- Rerank top-k to top-n for accuracy
    

#### **3.6.3 Security Considerations**

- Document access permissions & role-based retrieval
    
- Sensitive data detection & redaction
    
- Enforce audit logs for retrieval transparency
    
- Validation/verification step in pipeline
    
- Prevent prompt-level leakage of private information
    

---

---

## **Choose what to generate next**

a) Section **4 – Active Recall** (flashcards, quizzes, spaced repetition)  
b) **Visual Architecture Posters** for printing  
c) **Comparison tables for vector databases & embeddings**  
d) **Full design patterns workbook**  
e) **Turn all content into a printable PDF manual**

**Reply with: a / b / c / d / e / all** ✨