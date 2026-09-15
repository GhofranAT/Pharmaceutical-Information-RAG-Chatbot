# Pharmaceutical Information RAG Chatbot
## Modren Data Engineering for Advanced AI Systems, [SDAIA Academy](https://github.com/SDAIAAcademy)

## Project Overview

This project develops a **Retrieval-Augmented Generation (RAG) chatbot** for accessing and answering questions about pharmaceutical products using a collection of structured JSON drug records.

The system combines **semantic search, text embeddings, and a Large Language Model (LLM)** to provide users with relevant pharmaceutical information without requiring the LLM to rely solely on its internal knowledge. Instead, when a user asks a question, the system first retrieves the most relevant pharmaceutical records from the dataset and then provides those records as context to the LLM, which generates the final response.



## Dataset

The knowledge base consists of pharmaceutical drug records stored in JSON files. Each record contains information such as:

* Registration number
* Trade name
* Generic name
* Strength and dosage
* Administration route
* Pharmaceutical form
* Package size
* Legal classification
* Shelf life
* Storage conditions
* Manufacturer
* Marketing company
* Patient Information Leaflet (PIL) in English
* Patient Information Leaflet (PIL) in Arabic
* Summary of Product Characteristics (SPC)

The JSON records are converted into text so that they can be processed by the embedding model and used as retrieval context.

## RAG Pipeline

The system follows five main stages:

**1. Data Loading**

The pharmaceutical JSON files are loaded and converted into individual drug documents.

**2. Text Representation**

Each drug's structured information, PIL content, and SPC information are combined into a textual representation.

**3. Embedding Generation**

The `all-MiniLM-L6-v2` Sentence Transformer model converts each pharmaceutical document into a numerical vector (embedding).

**4. Semantic Retrieval**

When the user submits a question, the question is also converted into an embedding. The system calculates the **cosine similarity** between the question embedding and all pharmaceutical document embeddings. The documents with the highest similarity scores are retrieved as the most relevant context.

**5. Response Generation**

The retrieved pharmaceutical information is passed to an LLM through OpenRouter. The LLM uses the retrieved context to generate an answer while being instructed not to invent information that is not present in the available documents.

## System Architecture

```text
                    Pharmaceutical JSON Files
                              │
                              ▼
                       Data Processing
                              │
                              ▼
                    Text Representation
                              │
                              ▼
                    Document Embeddings
                 Sentence Transformer Model
                    (all-MiniLM-L6-v2)
                              │
                              ▼
User Question ───────► Query Embedding
                              │
                              ▼
                    Cosine Similarity
                              │
                              ▼
                     Top-K Documents
                              │
                              ▼
                    Retrieved Context
                              │
                              ▼
                    OpenRouter LLM
                              │
                              ▼
                     Generated Answer
```


