# Research Paper RAG Assistant

An end-to-end Retrieval-Augmented Generation (RAG) pipeline for scientific paper question answering, using SentenceTransformers, FAISS, Qwen, and retrieval evaluation.

## Project Overview

This project demonstrates how to build a document-based AI research assistant that answers questions about scientific papers without fine-tuning a large language model.

Instead of relying solely on the LLM's pretrained knowledge, the system retrieves relevant passages from a research paper and provides them as context to generate more document-grounded answers.

The project was developed in **Google Colab** using open-source models and libraries.

## System Architecture

The RAG pipeline consists of the following steps:

1. **Document Processing:** Extract text from a scientific PDF and split it into overlapping chunks.
2. **Embedding Generation:** Convert text chunks into semantic vector representations using SentenceTransformers.
3. **Vector Search:** Store embeddings in FAISS and retrieve relevant chunks based on user queries.
4. **Answer Generation:** Provide retrieved context to Qwen2.5-0.5B-Instruct to generate answers.
5. **Evaluation:** Measure retrieval performance and experiment with improvements.
6. **Source Attribution:** Display the document pages associated with retrieved passages.

**Workflow:**

PDF → Text Extraction → Chunking → Embeddings → FAISS Index → Relevant Chunks → Qwen LLM → Answer + Retrieved Sources

## Technologies Used

| Technology | Purpose |
|---|---|
| Python | Core development |
| Google Colab | Development environment |
| PyMuPDF | PDF text extraction |
| SentenceTransformers | Semantic embeddings |
| all-MiniLM-L6-v2 | Embedding model |
| FAISS | Vector similarity search |
| Qwen2.5-0.5B-Instruct | Answer generation |
| Hugging Face Transformers | LLM inference |
| BM25 | Experimental keyword retrieval |
| Gradio | Experimental web interface |

## Implementation

### Stage 1 — PDF Processing

- Extracted text from a scientific research paper.
- Divided the document into 95 overlapping text chunks.
- Preserved page numbers for source tracking.
- Saved processed chunks in `chunks.json`.

### Stage 2 — Semantic Retrieval

- Generated dense vector embeddings using `all-MiniLM-L6-v2`.
- Built a FAISS similarity-search index.
- Implemented top-k retrieval to identify relevant passages for user questions.

### Stage 3 — LLM Integration

- Integrated Qwen2.5-0.5B-Instruct.
- Combined retrieved document passages with user questions.
- Generated natural-language answers using the retrieved context.

### Stage 4 — Evaluation and Optimization

Evaluated retrieval using five development questions and manually identified relevant passages.

The evaluation used:

- **Hit@5:** Whether at least one relevant passage appeared in the top five results.
- **Recall@5:** Proportion of labeled relevant passages retrieved in the top five.
- **MRR@5:** Ranking quality based on the first relevant result.

### Retrieval Results

| Metric | Initial FAISS | Improved Retrieval |
|---|---:|---:|
| Hit@5 | 0.80 | 1.00 |
| Recall@5 | 0.80 | 1.00 |
| MRR@5 | 0.70 | 0.90 |

The improved retrieval approach included a paper-specific conclusion-section retrieval rule. Query rewriting and hybrid BM25/FAISS retrieval were also explored.

**Evaluation limitation:** These results were obtained on a small development set of five questions, with provisional relevance labels. They are not representative of performance on unseen documents or questions.

### Stage 5 — User Interface (Work in Progress)

Developed an initial Gradio interface to allow users to interact with the RAG pipeline through a web browser.

The interface has not yet been successfully launched end to end because of dependency compatibility issues.

## Repository Structure

```text
research-paper-rag-assistant/
├── LICENSE
├── README.md
├── RAG_project.ipynb
└── chunks.json
```

## Running the Project

1. Open `RAG_project.ipynb` in Google Colab.
2. Install the required dependencies using the notebook setup cells.
3. Upload the research PDF if required by the notebook.
4. Execute the notebook cells in sequence.
5. Ask questions about the document using the implemented RAG functions.

The project currently runs as a notebook-based prototype rather than a standalone deployed application.

## Key Learnings

This project provided hands-on experience with:

- Building an end-to-end RAG pipeline.
- Processing and indexing unstructured scientific documents.
- Generating semantic embeddings and performing vector similarity search.
- Integrating an open-source LLM without fine-tuning.
- Evaluating retrieval performance using standard information retrieval metrics.
- Experimenting with retrieval improvements.
- Tracking retrieved sources and identifying limitations in citation reliability.

## Current Limitations

- Evaluated on a single scientific paper and a small question set.
- Retrieval improvements include paper-specific heuristics.
- Retrieved page attribution does not guarantee that every generated claim is supported.
- The Gradio interface remains unfinished.
- Performance and reliability on larger document collections have not been established.

## Future Improvements

- Complete the Gradio web interface.
- Support uploading and querying multiple PDFs.
- Expand evaluation with independently labeled questions.
- Implement claim-level citation verification.
- Add retrieval reranking and more robust hybrid search.
- Package the application for reproducible deployment.

## Project Goal

Demonstrate practical experience in building, evaluating, and improving a Retrieval-Augmented Generation system using open-source AI technologies.

The project focuses on the complete workflow from document ingestion to retrieval, grounded answer generation, and evaluation.
