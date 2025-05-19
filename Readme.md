# Question Answering System Comparison

## Overview

This project compares three QA approaches on the SQuAD dataset:

- **Extractive QA**: Uses a BERT model fine-tuned on SQuAD to select answer spans.  
- **Generative QA**: Uses Gemini 2.0 and GPT-4.0 to generate free-form answers.  
- **Retrieval-Augmented Generation (RAG)**: Retrieves top-5 passages via FAISS (with `all-MiniLM-L6-v2` embeddings) and generates answers with a generative model.

Each approach is evaluated on Exact Match, F1, ROUGE-L, Recall@5, MRR@5, and throughput (QPS).

## Methodology

**A. Dataset**  
- **SQuAD**: >100 000 question–answer pairs from Wikipedia.  
- **Splits**:  
  - In-domain (val10): 10 questions from the validation set.  
  - Out-of-domain (train10): 10 questions from the training set.

**B. Extractive QA**  
- **Model**: `huggingface-course/bert-finetuned-squad`  
- **Approach**: Predicts start/end token indices for answer spans given question+context.

**C. Generative QA**  
- **Models**: Gemini 2.0, GPT-4.0  
- **Approach**: Generates answers from the question prompt without retrieval.

**D. Retrieval-Augmented Generation (RAG)**  
1. **Retrieval**  
   - **Engine**: FAISS with `all-MiniLM-L6-v2` embeddings via LangChain.  
   - Retrieves top 5 relevant passages per query.  
2. **Generation**  
   - Uses Gemini 2.0 or GPT-4.0 to generate answers conditioned on retrieved passages.

**E. Embedding Techniques**  
- **Library**: LangChain + HuggingFaceEmbeddings  
- **Model**: `sentence-transformers/all-MiniLM-L6-v2`  
- **Storage**: Dense vectors indexed in FAISS for nearest-neighbor search.

**F. Evaluation Metrics**  
- **Exact Match (EM)**: Word-for-word match rate.  
- **F1 Score**: Token-level harmonic mean of precision and recall.  
- **ROUGE-L**: Longest common subsequence measure.  
- **Recall@5**: Fraction of cases where the correct passage is in top 5 (RAG).  
- **MRR@5**: Mean reciprocal rank of the first correct passage (RAG).  
- **Throughput (QPS)**: Queries processed per second.  
