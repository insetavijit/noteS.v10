## Step 1: Environment Setup

### What You're Building
Setting up your Python environment with all necessary dependencies and API credentials.

### Why This Matters
Before writing any code, you need the right tools installed. This step ensures you have access to:
- Document loaders (for PDFs and text files)
- Text processing utilities (for chunking)
- Embedding models (for vectorization)
- Vector databases (for storage and retrieval)
- LLM integration (for answer generation)

### What You'll Achieve
✅ A clean virtual environment with all dependencies  
✅ Secure API key management  
✅ Ready-to-use project structure

### Implementation

```bash
# Create a new project directory
mkdir rag_qa_system
cd rag_qa_system

# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate

# Install required packages
pip install langchain openai pypdf faiss-cpu python-dotenv
```

Create a `.env` file in your project root:

```bash
# .env file
OPENAI_API_KEY=your_api_key_here
```

Create a basic folder structure:

```
rag_qa_system/
├── .env
├── documents/          # Place your PDF/TXT files here
├── main.py            # Your main script
└── requirements.txt   # Dependency list
```

**Test Your Setup:**
```python
# test_setup.py
import openai
from dotenv import load_dotenv
import os

load_dotenv()
print("API Key loaded:", os.getenv("OPENAI_API_KEY")[:10] + "...")
print("✅ Environment ready!")
```

---

## Step 2: Document Ingestion

### What You're Building
A function that loads documents from disk and extracts their text content.

### Why This Matters
Your RAG system needs raw text to work with. Different file formats require different parsing strategies. This component handles the complexity of reading PDFs and text files uniformly.

### What You'll Achieve
✅ Load PDF files and extract all text  
✅ Load TXT files with proper encoding  
✅ Handle multiple document formats seamlessly  
✅ Return standardized document objects

### Implementation

```python
# main.py
import os
from dotenv import load_dotenv
from langchain.document_loaders import PyPDFLoader, TextLoader

# Load environment variables
load_dotenv()

def load_document(file_path):
    """
    Load a document and extract its text content.
    
    Args:
        file_path (str): Path to the document file
        
    Returns:
        list: List of Document objects containing text and metadata
        
    Raises:
        ValueError: If file format is not supported
    """
    print(f"📄 Loading document: {file_path}")
    
    # Determine loader based on file extension
    if file_path.endswith('.pdf'):
        loader = PyPDFLoader(file_path)
        print("   Using PDF loader...")
    elif file_path.endswith('.txt'):
        loader = TextLoader(file_path, encoding='utf-8')
        print("   Using Text loader...")
    else:
        raise ValueError(f"Unsupported file format: {file_path}")
    
    # Load and return documents
    documents = loader.load()
    print(f"   ✅ Loaded {len(documents)} page(s)")
    return documents

# Test the function
if __name__ == "__main__":
    # Place a test PDF or TXT in the documents/ folder
    docs = load_document("documents/sample.pdf")
    print(f"\nFirst 200 characters:\n{docs[0].page_content[:200]}...")
```

**What's Happening:**
- `PyPDFLoader` extracts text from each PDF page separately
- `TextLoader` reads plain text files with UTF-8 encoding
- Each page/section becomes a `Document` object with `.page_content` and `.metadata`

---

## Step 3: Text Chunking

### What You're Building
A text splitter that breaks long documents into smaller, semantically meaningful pieces.
### Why This Matters
- **Embedding models have token limits** (typically 8,192 tokens for OpenAI)
- **Smaller chunks improve retrieval precision** — you want specific paragraphs, not entire chapters
- **Overlapping chunks preserve context** at boundaries between splits
### What You'll Achieve
✅ Split documents into 500-1000 character chunks  
✅ Add 100-200 character overlap to preserve context  
✅ Maintain paragraph and sentence boundaries when possible  
✅ Prepare text for efficient embedding
### Implementation

```python
from langchain.text_splitter import RecursiveCharacterTextSplitter

def chunk_documents(documents, chunk_size=800, chunk_overlap=150):
    """
    Split documents into smaller chunks for embedding.
    
    Args:
        documents (list): List of Document objects
        chunk_size (int): Maximum characters per chunk
        chunk_overlap (int): Characters to overlap between chunks
        
    Returns:
        list: List of chunked Document objects
    """
    print(f"\n✂️  Chunking documents (size={chunk_size}, overlap={chunk_overlap})")
    
    # Create text splitter with smart splitting
    # Tries to split on: \n\n, \n, space (in that order)
    text_splitter = RecursiveCharacterTextSplitter(
        chunk_size=chunk_size,
        chunk_overlap=chunk_overlap,
        length_function=len,
        separators=["\n\n", "\n", " ", ""]  # Priority order for splitting
    )
    
    # Split all documents
    chunks = text_splitter.split_documents(documents)
    print(f"   ✅ Created {len(chunks)} chunks")
    
    return chunks

# Test the function
if __name__ == "__main__":
    docs = load_document("documents/sample.pdf")
    chunks = chunk_documents(docs)
    
    # Inspect a chunk
    print(f"\nSample chunk #{5}:")
    print(f"Length: {len(chunks[4].page_content)} characters")
    print(f"Content preview:\n{chunks[4].page_content[:300]}...")
```

**What's Happening:**
- `RecursiveCharacterTextSplitter` tries to split on double newlines first (paragraphs)
- If chunks are still too large, it splits on single newlines (sentences)
- If still too large, it splits on spaces (words)
- Overlap ensures that context isn't lost at chunk boundaries

---

## Step 4: Embedding Generation

### What You're Building
A vector representation of each text chunk using OpenAI's embedding model.

### Why This Matters
Embeddings convert text into numerical vectors that capture semantic meaning. Similar concepts produce similar vectors, enabling semantic search (e.g., "automobile" finds "car" even without exact keyword match).

### What You'll Achieve
✅ Convert text chunks to 1536-dimensional vectors  
✅ Enable semantic similarity comparisons  
✅ Prepare data for vector database storage

### Implementation

```python
from langchain.embeddings import OpenAIEmbeddings

def create_embeddings():
    """
    Initialize the embedding model.
    
    Returns:
        OpenAIEmbeddings: Configured embedding model
    """
    print("\n🔢 Initializing embedding model (OpenAI text-embedding-ada-002)")
    
    embeddings = OpenAIEmbeddings(
        model="text-embedding-ada-002",  # Latest embedding model
        openai_api_key=os.getenv("OPENAI_API_KEY")
    )
    
    print("   ✅ Embedding model ready")
    return embeddings

# Test the function
if __name__ == "__main__":
    docs = load_document("documents/sample.pdf")
    chunks = chunk_documents(docs)
    embeddings = create_embeddings()
    
    # Test embedding a single chunk
    test_vector = embeddings.embed_query("What is machine learning?")
    print(f"\nTest embedding dimensions: {len(test_vector)}")
    print(f"First 5 values: {test_vector[:5]}")
```

**What's Happening:**
- `text-embedding-ada-002` is OpenAI's cost-effective, high-quality embedding model
- Each chunk gets converted to a 1536-dimensional vector
- Similar chunks will have vectors close together in vector space
- Cost: ~$0.0001 per 1,000 tokens

---

## Step 5: Vector Database Creation

### What You're Building
A FAISS vector database that stores embeddings and enables fast similarity search.

### Why This Matters
You need to store thousands of vectors and query them efficiently. FAISS (Facebook AI Similarity Search) provides in-memory vector storage with millisecond search times.

### What You'll Achieve
✅ Store all chunk embeddings in a searchable database  
✅ Enable fast k-nearest-neighbor search  
✅ Support semantic retrieval (find similar chunks)  
✅ Persist the database to disk for reuse

### Implementation

```python
from langchain.vectorstores import FAISS

def create_vector_store(chunks, embeddings, save_path="vector_store"):
    """
    Create and save a FAISS vector database.
    
    Args:
        chunks (list): List of text chunks
        embeddings (OpenAIEmbeddings): Embedding model
        save_path (str): Directory to save the database
        
    Returns:
        FAISS: Vector store object
    """
    print(f"\n💾 Creating vector store with {len(chunks)} chunks")
    
    # Create FAISS index from documents
    vectorstore = FAISS.from_documents(
        documents=chunks,
        embedding=embeddings
    )
    
    # Save to disk for future use
    vectorstore.save_local(save_path)
    print(f"   ✅ Vector store saved to '{save_path}/'")
    
    return vectorstore

def load_vector_store(embeddings, load_path="vector_store"):
    """
    Load an existing FAISS vector database.
    
    Args:
        embeddings (OpenAIEmbeddings): Embedding model
        load_path (str): Directory containing the database
        
    Returns:
        FAISS: Loaded vector store
    """
    print(f"\n📂 Loading vector store from '{load_path}/'")
    vectorstore = FAISS.load_local(load_path, embeddings)
    print("   ✅ Vector store loaded")
    return vectorstore

# Test the function
if __name__ == "__main__":
    docs = load_document("documents/sample.pdf")
    chunks = chunk_documents(docs)
    embeddings = create_embeddings()
    vectorstore = create_vector_store(chunks, embeddings)
    
    # Test similarity search
    query = "What is the main topic?"
    results = vectorstore.similarity_search(query, k=3)
    
    print(f"\n🔍 Top 3 results for: '{query}'")
    for i, doc in enumerate(results, 1):
        print(f"\nResult {i}:")
        print(doc.page_content[:200] + "...")
```

**What's Happening:**
- `FAISS.from_documents()` embeds all chunks and builds an index
- The index uses efficient algorithms for nearest-neighbor search
- `similarity_search(query, k=3)` finds the 3 most similar chunks
- Saving to disk lets you reuse the index without re-embedding

---

## Step 6: Question Answering Chain

### What You're Building
The final component that ties everything together: retrieval + answer generation.

### Why This Matters
This is where RAG happens. The system retrieves relevant chunks, then uses an LLM to synthesize a coherent answer grounded in the retrieved context.

### What You'll Achieve
✅ Retrieve top-k most relevant chunks for any question  
✅ Pass retrieved context to GPT for answer generation  
✅ Get accurate, grounded answers (not hallucinations)  
✅ Build a complete, interactive Q&A system

### Implementation

```python
from langchain.chains import RetrievalQA
from langchain.chat_models import ChatOpenAI

def create_qa_chain(vectorstore):
    """
    Create a question-answering chain with retrieval.
    
    Args:
        vectorstore (FAISS): Vector database for retrieval
        
    Returns:
        RetrievalQA: Q&A chain object
    """
    print("\n🤖 Creating Q&A chain with GPT-4")
    
    # Initialize language model
    llm = ChatOpenAI(
        model_name="gpt-4",  # or "gpt-3.5-turbo" for faster/cheaper
        temperature=0,  # Deterministic outputs
        openai_api_key=os.getenv("OPENAI_API_KEY")
    )
    
    # Create retrieval-based Q&A chain
    qa_chain = RetrievalQA.from_chain_type(
        llm=llm,
        chain_type="stuff",  # "stuff" = put all retrieved docs in prompt
        retriever=vectorstore.as_retriever(search_kwargs={"k": 3}),
        return_source_documents=True  # Include retrieved chunks in response
    )
    
    print("   ✅ Q&A chain ready")
    return qa_chain

def ask_question(qa_chain, question):
    """
    Ask a question and get an answer with sources.
    
    Args:
        qa_chain (RetrievalQA): Q&A chain
        question (str): User's question
        
    Returns:
        dict: Answer and source documents
    """
    print(f"\n❓ Question: {question}")
    print("   🔍 Retrieving relevant chunks...")
    
    result = qa_chain({"query": question})
    
    print(f"\n💬 Answer:\n{result['result']}")
    print(f"\n📚 Based on {len(result['source_documents'])} source chunks")
    
    return result

# Complete workflow
if __name__ == "__main__":
    # 1. Load document
    docs = load_document("documents/sample.pdf")
    
    # 2. Chunk text
    chunks = chunk_documents(docs)
    
    # 3. Create embeddings
    embeddings = create_embeddings()
    
    # 4. Create or load vector store
    try:
        vectorstore = load_vector_store(embeddings)
    except:
        vectorstore = create_vector_store(chunks, embeddings)
    
    # 5. Create Q&A chain
    qa_chain = create_qa_chain(vectorstore)
    
    # 6. Interactive Q&A loop
    print("\n" + "="*60)
    print("🚀 RAG System Ready! Ask questions about your document.")
    print("   Type 'quit' to exit.")
    print("="*60)
    
    while True:
        question = input("\nYour question: ").strip()
        
        if question.lower() in ['quit', 'exit', 'q']:
            print("\n👋 Goodbye!")
            break
            
        if not question:
            continue
            
        result = ask_question(qa_chain, question)
        
        # Show source snippets
        print("\n📄 Source snippets:")
        for i, doc in enumerate(result['source_documents'], 1):
            print(f"\n[{i}] {doc.page_content[:150]}...")
```

**What's Happening:**
- `RetrievalQA` combines retrieval + LLM generation
- For each question, it:
  1. Embeds the question
  2. Finds top-3 similar chunks
  3. Passes chunks + question to GPT
  4. GPT synthesizes an answer using only retrieved context
- `return_source_documents=True` lets you see which chunks were used

---

## Step 7: Complete System with Error Handling

### What You're Building
A production-ready script with proper error handling, logging, and user experience.

### Why This Matters
Real-world systems need to handle failures gracefully: missing files, API errors, invalid queries, etc.

### What You'll Achieve
✅ Robust error handling for all components  
✅ Helpful error messages for debugging  
✅ Clean user interface for Q&A interaction  
✅ Ready for real-world use

### Implementation

```python
# complete_rag_system.py
import os
import sys
from dotenv import load_dotenv
from langchain.document_loaders import PyPDFLoader, TextLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import FAISS
from langchain.chains import RetrievalQA
from langchain.chat_models import ChatOpenAI

class RAGSystem:
    """Complete RAG Q&A system with error handling."""
    
    def __init__(self, document_path, vector_store_path="vector_store"):
        """Initialize the RAG system."""
        load_dotenv()
        self.document_path = document_path
        self.vector_store_path = vector_store_path
        self.vectorstore = None
        self.qa_chain = None
        
        # Validate API key
        if not os.getenv("OPENAI_API_KEY"):
            raise ValueError("❌ OPENAI_API_KEY not found in .env file")
    
    def load_document(self):
        """Load document with error handling."""
        try:
            print(f"📄 Loading: {self.document_path}")
            
            if not os.path.exists(self.document_path):
                raise FileNotFoundError(f"File not found: {self.document_path}")
            
            if self.document_path.endswith('.pdf'):
                loader = PyPDFLoader(self.document_path)
            elif self.document_path.endswith('.txt'):
                loader = TextLoader(self.document_path, encoding='utf-8')
            else:
                raise ValueError("Unsupported format. Use .pdf or .txt")
            
            docs = loader.load()
            print(f"   ✅ Loaded {len(docs)} page(s)")
            return docs
            
        except Exception as e:
            print(f"   ❌ Error loading document: {str(e)}")
            sys.exit(1)
    
    def chunk_documents(self, docs):
        """Chunk documents with validation."""
        try:
            print("✂️  Chunking text...")
            splitter = RecursiveCharacterTextSplitter(
                chunk_size=800,
                chunk_overlap=150,
                separators=["\n\n", "\n", " ", ""]
            )
            chunks = splitter.split_documents(docs)
            print(f"   ✅ Created {len(chunks)} chunks")
            return chunks
        except Exception as e:
            print(f"   ❌ Error chunking: {str(e)}")
            sys.exit(1)
    
    def setup_vector_store(self, chunks):
        """Create or load vector store."""
        try:
            embeddings = OpenAIEmbeddings()
            
            # Try loading existing store
            if os.path.exists(self.vector_store_path):
                print(f"📂 Loading existing vector store...")
                self.vectorstore = FAISS.load_local(
                    self.vector_store_path, 
                    embeddings
                )
                print("   ✅ Loaded from disk")
            else:
                # Create new store
                print(f"💾 Creating vector store...")
                self.vectorstore = FAISS.from_documents(chunks, embeddings)
                self.vectorstore.save_local(self.vector_store_path)
                print("   ✅ Created and saved")
                
        except Exception as e:
            print(f"   ❌ Error with vector store: {str(e)}")
            sys.exit(1)
    
    def setup_qa_chain(self):
        """Initialize Q&A chain."""
        try:
            print("🤖 Setting up Q&A chain...")
            llm = ChatOpenAI(model_name="gpt-3.5-turbo", temperature=0)
            
            self.qa_chain = RetrievalQA.from_chain_type(
                llm=llm,
                chain_type="stuff",
                retriever=self.vectorstore.as_retriever(search_kwargs={"k": 3}),
                return_source_documents=True
            )
            print("   ✅ Ready for questions")
            
        except Exception as e:
            print(f"   ❌ Error setting up Q&A: {str(e)}")
            sys.exit(1)
    
    def ask(self, question):
        """Ask a question and return answer with sources."""
        try:
            if not self.qa_chain:
                raise RuntimeError("System not initialized. Call build() first.")
            
            result = self.qa_chain({"query": question})
            return result
            
        except Exception as e:
            return {"result": f"Error: {str(e)}", "source_documents": []}
    
    def build(self):
        """Build the complete RAG pipeline."""
        print("\n" + "="*60)
        print("🚀 Building RAG System")
        print("="*60 + "\n")
        
        docs = self.load_document()
        chunks = self.chunk_documents(docs)
        self.setup_vector_store(chunks)
        self.setup_qa_chain()
        
        print("\n" + "="*60)
        print("✅ System ready!")
        print("="*60 + "\n")
    
    def interactive_mode(self):
        """Run interactive Q&A session."""
        print("💬 Interactive Mode - Type 'quit' to exit\n")
        
        while True:
            question = input("❓ Your question: ").strip()
            
            if question.lower() in ['quit', 'exit', 'q']:
                print("\n👋 Goodbye!")
                break
            
            if not question:
                continue
            
            print("\n🔍 Searching...")
            result = self.ask(question)
            
            print(f"\n💬 Answer:\n{result['result']}\n")
            
            if result['source_documents']:
                print("📚 Sources:")
                for i, doc in enumerate(result['source_documents'], 1):
                    print(f"  [{i}] {doc.page_content[:100]}...")
            
            print("\n" + "-"*60 + "\n")

# Main execution
if __name__ == "__main__":
    # Configuration
    DOCUMENT_PATH = "documents/sample.pdf"  # Change this to your document
    
    # Build system
    rag = RAGSystem(DOCUMENT_PATH)
    rag.build()
    
    # Start interactive Q&A
    rag.interactive_mode()
```

**What's Happening:**
- `RAGSystem` class encapsulates all components
- Each method has try-except blocks for graceful error handling
- `build()` runs the entire pipeline in sequence
- `interactive_mode()` provides a clean user interface
- Errors show helpful messages instead of cryptic stack traces

---

## Testing Your System

Create a test document (`documents/sample.txt`):

```text
Machine Learning Fundamentals

Machine learning is a subset of artificial intelligence that enables 
computers to learn from data without explicit programming. The main 
types include supervised learning, unsupervised learning, and 
reinforcement learning.

Supervised learning uses labeled training data to predict outcomes. 
Common algorithms include linear regression, decision trees, and 
neural networks. Applications include spam detection and image 
recognition.

Deep learning is a specialized branch using neural networks with 
multiple layers. It excels at processing images, speech, and text.
```

Run the system and test with questions:
- "What is machine learning?"
- "What are the main types of machine learning?"
- "Name some supervised learning algorithms"

---

## Next Steps

Once your system works:

1. **Experiment with parameters:**
   - Try different chunk sizes (500, 1000, 1500)
   - Adjust `k` values (retrieve 3, 5, or 10 chunks)
   - Test different LLMs (gpt-4 vs gpt-3.5-turbo)

2. **Add features:**
   - Metadata filtering (by date, author, source)
   - Multi-document support
   - Chat history for follow-up questions

3. **Optimize costs:**
   - Use HuggingFace embeddings (free)
   - Cache embeddings to avoid re-processing
   - Use cheaper models for simple queries

4. **Production hardening:**
   - Add logging with `logging` module
   - Implement rate limiting
   - Deploy with persistent database (Pinecone/Weaviate)

🎉 **Congratulations!** You've built a complete RAG system from scratch!