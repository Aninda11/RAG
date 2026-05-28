# Design and Evaluation of a Retrieval-Augmented Generation System
## A Phishing Guideline Case Study

This repository contains the implementation of a lightweight **Retrieval-Augmented Generation (RAG)** system designed for **cybersecurity guideline generation**, with a specific focus on **phishing-related practitioner queries**. The project was developed as part of an **Advanced AI / Machine Learning** assignment and demonstrates how retrieval, language generation, and trustworthiness evaluation can be combined in a practical domain-specific setting.

The system uses a phishing-focused PDF knowledge base, retrieves the most relevant chunks using dense embeddings and FAISS, and generates short cybersecurity guidelines using an open-source language model. The generated outputs are then evaluated using **RAGAS-style reference-free metrics**: **context relevance**, **answer relevance**, and **faithfulness**.

---

## Project Motivation

Large language models can generate fluent text, but in domain-specific settings they may still produce vague, incomplete, or hallucinated outputs. This is especially risky in cybersecurity, where practitioners may require concise and trustworthy guidance. Retrieval-Augmented Generation addresses this problem by grounding responses in retrieved external knowledge rather than relying only on the model’s internal parameters.

In this project, phishing was selected as the case-study domain because it is a common, practical, and high-impact cybersecurity problem. The goal was not to build a production-ready cybersecurity assistant, but to implement a lightweight and explainable RAG pipeline that demonstrates the core workflow of retrieval, generation, and evaluation.

---

## Objectives

The main objectives of this project are:

- design a domain-specific RAG system for phishing-related cybersecurity guideline generation
- construct a phishing-focused knowledge base with source-style references
- implement document chunking, embedding generation, and vector retrieval
- generate concise phishing-related guidelines using an open-source language model
- evaluate the system using:
  - **Context Relevance**
  - **Answer Relevance**
  - **Faithfulness**
- analyze limitations, trustworthiness concerns, and possible improvements

---

## System Overview

The implemented pipeline follows these stages:

1. **Knowledge Base Preparation**  
   A phishing-focused PDF knowledge base was created containing 50 question-answer style entries with source tags.

2. **PDF Processing and Chunking**  
   Text is extracted from the PDF and split into overlapping chunks.

3. **Embedding Generation**  
   Each chunk is converted into a dense vector using the `all-MiniLM-L6-v2` sentence-transformer model.

4. **Vector Indexing with FAISS**  
   All embeddings are stored in a FAISS vector index for efficient similarity-based retrieval.

5. **Retrieval**  
   For a practitioner query, the system retrieves the top-3 most relevant chunks.

6. **Generation**  
   The retrieved context and user query are combined into a prompt and passed to a lightweight open-source generation model (`distilgpt2`) to produce three concise guidelines.

7. **Evaluation**  
   The outputs are evaluated using **context relevance**, **answer relevance**, and **faithfulness**.

---

## Repository Structure

```text
.
├── Copy_of_Colab(1).ipynb
├── phishing_knowledge_base_50qna.pdf
├── slide_rag_pipeline.png
├── Reportfor3.pdf
├── 1.png
├── 2.png
└── README.md
```

---

## Main Technologies Used

- Python
- Google Colab
- SentenceTransformers
- FAISS
- Transformers
- PyPDF
- NumPy
- Pandas
- Matplotlib
- Scikit-learn

---

## Installation

This project was built and tested in **Google Colab**.

```python
!pip -q install sentence-transformers faiss-cpu transformers pypdf
```

If you are running locally:

```bash
pip install sentence-transformers faiss-cpu transformers pypdf numpy pandas matplotlib scikit-learn
```

---

## How to Run

1. Open the notebook in **Google Colab**
2. Upload the required PDF file:
   - `phishing_knowledge_base_50qna.pdf`
3. Run the cells step by step from top to bottom
4. The notebook will:
   - load and process the PDF
   - create chunks
   - generate embeddings
   - build the FAISS index
   - retrieve top-3 relevant chunks
   - generate responses
   - compute evaluation metrics
   - display figures and summary tables

---

## Knowledge Base

The knowledge base is a phishing-focused PDF document created specifically for this project. It contains **50 question-answer style entries** covering topics such as phishing email identification, suspicious link handling, password compromise response, multi-factor authentication, spear-phishing awareness, credential theft response, phishing prevention controls, employee training, incident reporting, and technical controls such as filtering and verification.

The document also includes **source-style references** inspired by cybersecurity guidance materials, which makes the project more realistic and better aligned with a retrieval-based architecture.

---

## Retrieval and Generation Design

### Retrieval
- text extracted from PDF
- chunk size: fixed-size chunks with overlap
- embedding model: `all-MiniLM-L6-v2`
- vector store: `FAISS IndexFlatL2`
- top-k retrieval: `k = 3`

### Generation
- model: `distilgpt2`
- prompt includes:
  - retrieved context
  - practitioner query
  - instruction to generate 3 concise guidelines

This design was intentionally kept lightweight to make the project reproducible and easy to demonstrate in an educational setting.

---

## Evaluation

The project evaluates the system using three **RAGAS-style reference-free metrics**:

### 1. Context Relevance
Measures how relevant the retrieved chunks are to the practitioner’s query.

### 2. Answer Relevance
Measures how well the generated response addresses the user’s question.

### 3. Faithfulness
Measures how well the generated output remains grounded in the retrieved context.

These metrics are approximated using **cosine similarity** between embeddings of:
- the query
- the retrieved chunks
- the generated response

---

## Final Results

The system was evaluated on **10 practitioner-style phishing queries**.

### Average Scores
- **Context Relevance:** 0.564
- **Answer Relevance:** 0.587
- **Faithfulness:** 0.781

### Interpretation
- The **highest score was faithfulness**, suggesting that the outputs were relatively well grounded in the retrieved evidence.
- The lower **context relevance** suggests that retrieval was not always sharply focused.
- The lower **answer relevance** suggests that the generation model often produced responses that were somewhat general or less precisely matched to the specific query.

---

## Key Findings

- The system works as a **lightweight RAG baseline**
- It demonstrates the full pipeline of retrieval, generation, and evaluation
- The outputs were more **grounded** than they were **precise**
- The major weaknesses were limited retrieval focus and weak answer specificity

---

## Limitations

This project has several limitations:

- the knowledge base is relatively small and manually curated
- dense retrieval alone does not guarantee highly focused context
- `distilgpt2` is a lightweight model and produces weaker generation quality than stronger LLMs
- evaluation uses proxy metrics rather than human expert judgments
- the system is not intended for real-world deployment without human oversight

---

## Future Improvements

Possible improvements include:

- expanding the phishing knowledge base with more diverse real documents
- using stronger open-source LLMs
- improving prompt design for more structured outputs
- using hybrid retrieval or reranking
- adding source-aware or citation-aware output generation
- performing human evaluation in addition to RAGAS-style metrics

---

## Author

**Aninda Kumar Sharma**  
Student ID: *a1984174*

---

## License

This repository is for **academic and educational use only** unless otherwise specified.
