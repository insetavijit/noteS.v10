## 1. Constructor & Initialization

### `PyPDFLoader(file_path, password=None, extract_images=False)`

|Parameter|Type|Required|Default|Description|
|---|---|---|---|---|
|`file_path`|`str`|✅ Yes|—|Absolute or relative path to the PDF file|
|`password`|`str \| None`|No|`None`|Password for encrypted/protected PDFs|
|`extract_images`|`bool`|No|`False`|Extract embedded images as base64 metadata|

**Usage Example**

```python
from langchain_community.document_loaders import PyPDFLoader

# Basic usage
loader = PyPDFLoader("example.pdf")

# With password protection
loader = PyPDFLoader("secure.pdf", password="mypassword")

# With image extraction
loader = PyPDFLoader("report.pdf", extract_images=True)
```

---

## 2. Main Methods

|Method|Returns|Description|Use Case|
|---|---|---|---|
|`load()`|`List[Document]`|Loads entire PDF into memory at once|Small to medium PDFs (< 100 pages)|
|`lazy_load()`|`Iterator[Document]`|Streams pages one at a time|Large PDFs, memory-constrained environments|
|`__iter__()`|`Generator[Document]`|Enables `for page in loader:` syntax|Clean iteration in pipelines|

**Method Comparison**

```python
# Method 1: load() - All at once
documents = loader.load()
print(f"Loaded {len(documents)} pages")

# Method 2: lazy_load() - Iterator
for doc in loader.lazy_load():
    process(doc)  # Process one page at a time

# Method 3: Direct iteration
for page in loader:
    print(page.metadata['page'])
```

---

## 3. Document Object Structure

Each `Document` returned has:

### Attributes

|Attribute|Type|Description|Example|
|---|---|---|---|
|`page_content`|`str`|Extracted text from the page|`"Chapter 1: Introduction to Biology..."`|
|`metadata`|`dict`|File and page information|`{"source": "bio.pdf", "page": 0}`|

### Document Methods

```python
doc = documents[0]

# Access content
text = doc.page_content

# Access metadata
source = doc.metadata['source']
page_num = doc.metadata['page']

# String representation
print(doc)  # Shows content preview + metadata
```

---

## 4. Metadata Fields

|Key|Type|Description|Example Value|
|---|---|---|---|
|`source`|`str`|Original filename or path|`"NCERT_Biology_Ch1.pdf"`|
|`page`|`int`|Zero-indexed page number|`0` (first page)|
|`total_pages`|`int` (optional)|Total pages in PDF|`224`|
|`file_path`|`str` (optional)|Full file path|`"/docs/textbooks/bio.pdf"`|

**Accessing Metadata**

```python
for doc in documents:
    print(f"Page {doc.metadata['page'] + 1}/{doc.metadata.get('total_pages', '?')}")
    print(f"From: {doc.metadata['source']}")
```

---

## 5. Complete Workflow Examples

### Example 1: Basic PDF Loading

```python
from langchain_community.document_loaders import PyPDFLoader

# Load PDF
loader = PyPDFLoader("NCERT_Science_Class10.pdf")
documents = loader.load()

# Inspect results
print(f"Total pages: {len(documents)}")
print(f"First page preview:\n{documents[0].page_content[:300]}...")
print(f"Metadata: {documents[0].metadata}")
```

**Output:**

```
Total pages: 224
First page preview:
Chapter 1 - Chemical Reactions and Equations
A chemical reaction is a process in which one or more substances, 
the reactants, are converted to one or more different substances...
Metadata: {'source': 'NCERT_Science_Class10.pdf', 'page': 0, 'total_pages': 224}
```

---

### Example 2: Memory-Efficient Large PDF Processing

```python
from langchain_community.document_loaders import PyPDFLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter

loader = PyPDFLoader("large_textbook.pdf")
splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=100)

# Process page by page without loading entire PDF
chunks = []
for page in loader.lazy_load():
    page_chunks = splitter.split_documents([page])
    chunks.extend(page_chunks)
    
print(f"Created {len(chunks)} chunks from PDF")
```

---

### Example 3: Password-Protected PDF

```python
loader = PyPDFLoader("confidential_report.pdf", password="secure123")

try:
    documents = loader.load()
    print(f"Successfully loaded {len(documents)} pages")
except Exception as e:
    print(f"Error: {e}")  # Wrong password or corrupted file
```

---

### Example 4: Filtering Specific Pages

```python
loader = PyPDFLoader("textbook.pdf")
documents = loader.load()

# Extract only pages 10-20
selected_pages = [doc for doc in documents if 10 <= doc.metadata['page'] <= 20]

# Extract only pages containing specific keyword
keyword_pages = [doc for doc in documents if "mitochondria" in doc.page_content.lower()]

print(f"Found keyword in {len(keyword_pages)} pages")
```

---

## 6. Method Selection Guide

|Scenario|Recommended Method|Reason|
|---|---|---|
|PDF < 50 pages|`load()`|Fast, simple, low memory overhead|
|PDF > 100 pages|`lazy_load()`|Prevents memory issues|
|Real-time streaming|`for page in loader:`|Clean syntax, memory efficient|
|Need all pages upfront|`load()`|Allows immediate indexing/filtering|
|Pipeline integration|`lazy_load()`|Compatible with chunking/embedding streams|

---

## 7. Integration with RAG Pipeline

```python
from langchain_community.document_loaders import PyPDFLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_community.vectorstores import Chroma
from langchain_openai import OpenAIEmbeddings

# Step 1: Load PDF
loader = PyPDFLoader("knowledge_base.pdf")
documents = loader.load()

# Step 2: Split into chunks
splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=200
)
chunks = splitter.split_documents(documents)

# Step 3: Create vector store
embeddings = OpenAIEmbeddings()
vectorstore = Chroma.from_documents(chunks, embeddings)

# Step 4: Query
retriever = vectorstore.as_retriever()
results = retriever.get_relevant_documents("What is photosynthesis?")
```

---

## 8. Error Handling & Best Practices

```python
from pathlib import Path

def safe_load_pdf(file_path: str, password: str = None):
    """Robust PDF loading with error handling"""
    
    # Validate file exists
    if not Path(file_path).exists():
        raise FileNotFoundError(f"PDF not found: {file_path}")
    
    # Validate file extension
    if not file_path.lower().endswith('.pdf'):
        raise ValueError("File must be a PDF")
    
    # Load with error handling
    try:
        loader = PyPDFLoader(file_path, password=password)
        documents = loader.load()
        
        if not documents:
            raise ValueError("PDF contains no readable pages")
            
        return documents
        
    except Exception as e:
        print(f"Error loading PDF: {e}")
        return None

# Usage
docs = safe_load_pdf("report.pdf")
if docs:
    print(f"Successfully loaded {len(docs)} pages")
```

---

## 9. Performance Tips

|Tip|Impact|Implementation|
|---|---|---|
|Use `lazy_load()` for large files|🚀 High|`for page in loader.lazy_load():`|
|Filter pages early|⚡ Medium|Filter before chunking/embedding|
|Batch processing|🔄 Medium|Process pages in batches of 10-20|
|Cache loaded documents|💾 High|Store `documents` list for reuse|

---

## Summary

`PyPDFLoader` provides a production-ready API for extracting PDF content into structured `Document` objects with rich metadata — perfect for:

- ✅ RAG (Retrieval-Augmented Generation) pipelines
- ✅ Document Q&A systems
- ✅ Knowledge base construction
- ✅ Research paper analysis
- ✅ Educational content processing

---

## What's Next?

Choose your next API reference:

**A.** `RecursiveCharacterTextSplitter` — Intelligent text chunking  
**B.** `OpenAIEmbeddings` / `HuggingFaceEmbeddings` — Vector conversion  
**C.** `Chroma` / `FAISS` — Vector database operations  
**D.** `RetrievalQA` — Complete RAG chain setup

**Reply with:** A / B / C / D ✨