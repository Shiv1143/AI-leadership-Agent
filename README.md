## AI Leadership Insight Agent

This is a Retrieval-Augmented Generation (RAG) system built in a Google Colab notebook that acts as an AI-powered analyst for company leadership.
You feed it internal company documents — annual reports, quarterly earnings, strategy notes, operational updates — and it lets executives ask plain-English questions like "Which departments are underperforming?" or "What were the key risks last quarter?" and get back concise, factual answers that cite the exact source documents.
Under the hood, it chunks the documents into passages, converts them into vector embeddings using a sentence-transformer model, stores them in a FAISS index, and at query time retrieves the most relevant passages and passes them to Claude to generate a grounded answer. It ships with four realistic sample company documents so it runs immediately out of the box, plus a Gradio chat UI for live Q&A.
The core guarantee: the agent only answers from what's in your documents — it won't hallucinate facts it doesn't have.

### Quickstart

1. Open the notebook
Upload AI_Leadership_Insight_Agent.ipynb to Google Colab or click Open in Colab if hosted on GitHub.

2. Get an Open AI API key

3. Run all cells
The notebook will prompt you to enter your API key securely, then:
Install all dependencies automatically
Load the built-in sample documents
Build the vector index
Launch the Gradio chat UI

5. Ask questions
Use the chat interface or run individual question cells:
answer = ask_agent("What is our current revenue trend?")
print(answer)

### Sample Questions

The agent is designed to answer leadership questions such as:

Example Question
1. "What is our current revenue trend?"
2. "Which departments are underperforming and why?"
Risk
3. "What were the key risks highlighted in the last quarter?"
Strategy
4. "What are our top strategic priorities for 2025?"
People
5. "What are our current talent and retention challenges?"
Commercial
6. "What major deals did we close recently?"
Segment
7. "How is the Cloud Services division performing?"
Using Your Own Documents

Select your PDF / DOCX / TXT files

from google.colab import files
uploaded = files.upload()   # select your company documents

index rebuilds automatically

Supported formats: .pdf · .docx · .txt

### Configuration

All key parameters are set in one place at the top of the notebook:
CONFIG = {
    "model":           "gpt-4o-mini",  # LLM to use
    "max_tokens":      1024,                          # Max length of each answer
    "chunk_size":      800,                           # Characters per text chunk
    "chunk_overlap":   150,                           # Overlap between chunks
    "top_k":           5,                             # Chunks retrieved per query
    "embedding_model": "all-MiniLM-L6-v2",            # Sentence-transformer model
}

### Project Structure

AI_Leadership_Insight_Agent.ipynb
│
├── Step 1  — Install dependencies
├── Step 2  — Configuration & API key
├── Step 3  — Document ingestion (PDF / DOCX / TXT loaders + sample docs)
├── Step 4  — Chunking & embedding (FAISS index build)
├── Step 5  — Retrieval & generation (ask_agent function)
├── Step 6  — Pre-loaded leadership questions (6 examples)
├── Step 7  — Gradio interactive chat UI
├── Step 8  — Retrieval quality evaluator
└── Step 9  — Upload your own documents

### Requirements

1. Python 3.9+
2. Google Colab (free tier is sufficient)
3. OpenAI API key 
4. Internet connection (for package install and API calls)
5. All Python dependencies are installed automatically within the notebook:
openai · langchain · langchain-text-splitters · faiss-cpu
sentence-transformers · pypdf · python-docx · gradio · tiktoken

### Limitations
1. Context window — very long documents may exceed what fits in a single LLM call; chunking mitigates this but very niche facts buried deep in large files may not surface
2. In-memory index — the FAISS index is rebuilt each session; for persistence across sessions, swap to ChromaDB or Pinecone
