# PharmaAssist – RAG for Pharmaceutical Research

**PharmaAssist** is a Retrieval-Augmented Generation (RAG) application built to assist pharmaceutical researchers. It provides a reliable method to extract information and answer specific questions grounded in proprietary or uploaded PDF documents.

The application uses **LangChain** for pipeline orchestration, **Chroma** as the vector store, and leverages the high performance of the **Llama-3.3-70B-Versatile** model via the **Groq API** for rapid, context-aware responses.


## RAG Pipeline Flow

The system processes information in two key phases: Document Ingestion and Query Execution.

### Phase 1: Document Ingestion

* **PDF Loading:** PDF documents are loaded using the `PyPDFLoader`.
* **Text Splitting:** Text is segmented into small, overlapping chunks (size 100, overlap 50) using the `SentenceTransformersTokenTextSplitter`.
* **Embedding:** Text chunks are transformed into dense vector representations using the **HuggingFace `all-mpnet-base-v2`** embedding model.
* **Vector Storage:** These embeddings are persisted in the **Chroma** vector database (`pharma_db`) for efficient future retrieval.

### Phase 2: Query Execution

* **Retrieval:** The user's question is used to retrieve the **top-k (k=5)** most relevant document chunks from the Chroma database.
* **Context Construction:** The retrieved chunks are formatted and inserted, along with the original question, into a specialized `PROMPT_TEMPLATE`.
* **LLM Generation:** The complete prompt is passed to the **ChatGroq LLM (`llama-3.3-70b-versatile`)**.
* **Final Output:** The LLM's response is parsed and presented as the final, grounded answer to the user.

## Prerequisites

To deploy and run this application, you require:

* Python 3.8+
* A Groq API Key

## Installation

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/your-username/PharmaAssist.git](https://github.com/your-username/PharmaAssist.git)
    cd PharmaAssist
    ```

2.  **Install Dependencies:**
    A comprehensive list of required libraries:
    ```bash
    pip install langchain-groq langchain-chroma langchain-community langchain-core langchain-text-splitters gradio pypdf sentence-transformers
    ```

## Interface Workflow

1.  **API Key:** Enter your confidential Groq API Key into the designated password field.
2.  **Document Upload:** Upload one or more pharmaceutical PDF documents.
3.  **Query:** Enter your specific research question into the text box.
4.  **Answer:** The system will return an answer strictly based on the context of your uploaded documents.
