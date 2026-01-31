# Prompt Engineering & RAG Mini Project

## Overview
This project implements a small Retrieval-Augmented Generation (RAG) system for answering questions from company policy documents (Refund, Cancellation, Shipping). The system retrieves relevant document chunks and generates grounded answers using an open-source language model, while minimizing hallucinations through prompt design.

## Objective
- Build a working RAG pipeline  
- Design effective prompts  
- Reduce hallucinations  
- Evaluate and reason about model outputs  

## Tech Stack
- Python  
- LangChain  
- ChromaDB  
- HuggingFace Transformers  
- SentenceTransformers  
- Google Colab  

## Folder Structure
rag-mini-project/  
├── Notebook/  
│   └── rag_project.ipynb  
├── data/  
│   ├── Refund_Policy.docx  
│   ├── Cancellation_Policy.docx  
│   └── Shipping_Policy.docx  
└── README.md  

## Data Preparation
Documents are loaded automatically from the data folder using python-docx and converted into LangChain Document format.  

- Chunk size: 500 characters  
- Chunk overlap: 50 characters  

Reason:  
Balances semantic completeness with retrieval precision.

## RAG Architecture
1. Load documents  
2. Chunk documents  
3. Generate embeddings  
4. Store in Chroma vector database  
5. Retrieve top-k similar chunks  
6. Generate answer from retrieved context  
7. Persist vectors locally using ChromaDB  

## Prompt Engineering

### Prompt Version 1  
Simple context + question format.

### Prompt Version 2 (Final)

You are an AI assistant that answers strictly from company policy documents.

Rules:
1. Use ONLY the information in <context>.  
2. If answer not found, say:  
   Not found in the provided documents.  
3. Do NOT guess or use outside knowledge.  
4. Cite evidence.  

<context>  
{context}  
</context>  

Question:  
{question}  

Answer Format:  
- Answer:  
- Evidence:  

Improvement:  
Prompt V2 restricts the model to retrieved context and reduces hallucinations.

## Language Model
Open-source model: google/flan-t5-base  
Used via HuggingFace Transformers.

## Evaluation

Evaluation is implemented as a reusable function (run_evaluation).

Question | Expected | Output | Score  
What is the refund period? | 30 days | 30 days | ✅  
International shipping time | 10-15 days | 10-15 days | ✅  
Cancellation after shipping | No | No | ✅  
Are digital products refundable | No | No | ✅  
Do you ship to Germany | Not found | Partial | ⚠️  
Payment gateway used | Not found | Partial | ⚠️  

## Edge Case Handling
If no relevant information is found:

Not found in the provided documents.

## How to Run
1. Open rag_project.ipynb in Google Colab  
2. Install libraries  
3. Place policy DOCX files inside:  
   /content/drive/MyDrive/rag_project/data  
4. Run cells sequentially  

## Key Trade-offs
- Lightweight embeddings for speed vs accuracy  
- ChromaDB chosen for simple local persistence  
- No reranking for simplicity  
- Single-notebook design prioritizes clarity over modularity  

## Future Improvements
- Add reranking  
- Structured JSON outputs  
- Logging  

## Proud Of
Building complete RAG pipeline with grounding and prompt iteration.

## Next Improvements
- Add reranking  
- Output schema validation  
