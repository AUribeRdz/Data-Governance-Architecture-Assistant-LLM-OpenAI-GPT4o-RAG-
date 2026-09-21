# Data Governance Architecture Assistant (LLM + RAG)

A retrieval-augmented chatbot that answers data-governance architecture questions from a curated policy document, using GPT-4o, and declines when the document does not contain the answer.

## Overview

Data stewards and architects need answers that match current policy, not the model's general memory. This notebook retrieves relevant passages from a governance architecture PDF and instructs the model to answer only from that context. If the answer is not in the document, the assistant says so and points to a contact address.

## How it works

1. **Load.** `PyPDFLoader` reads `data-governance-architecture-patterns.pdf`.
2. **Chunk.** `RecursiveCharacterTextSplitter` with chunk size 1000 and overlap 200.
3. **Embed and store.** OpenAI `text-embedding-3-small` vectors are stored in a local, persisted Chroma database.
4. **Retrieve.** The top 3 chunks for each question.
5. **Generate.** A grounded prompt is sent to `gpt-4o` at temperature 0. The prompt requires technical, direct answers and defines the exact refusal sentence for out-of-scope questions.
6. **Cache.** An SQLite LLM cache avoids repeated calls during development.

## Tech stack

| Area | Tools |
| --- | --- |
| Language | Python, Jupyter Notebook |
| Orchestration | LangChain |
| Models | OpenAI `gpt-4o`, `text-embedding-3-small` |
| Vector store | Chroma (local) |

## Results

The saved notebook output shows the refusal path: for a sample question the retrieved context does not answer, the assistant returns the defined out-of-scope message instead of guessing.

## Run it

1. Create a `.env` file:

   ```text
   OPENAI_API_KEY=your-key-here
   ```

2. Place the source PDF in the notebook folder as `data-governance-architecture-patterns.pdf`. The document is not included in this repository.

3. Install dependencies and run:

   ```bash
   pip install langchain langchain-community langchain-openai langchain-chroma langchain-text-splitters pypdf python-dotenv jupyter
   jupyter notebook LMM_OpenAI_GPT4_RAG.ipynb
   ```

## Known limitations

- The source PDF is not included, so the notebook cannot be run as-is.
- Answers do not yet include citations, and there is no relevance-score filter before generation.
- There is no automated evaluation of retrieval quality or answer faithfulness.
- The local Chroma store and SQLite cache are sandbox choices; an enterprise deployment would use managed, access-controlled storage and a distributed cache.

## Next steps

- Return source page numbers (already available in the loader metadata) as citations.
- Add a relevance-score threshold before generation.
- Build a small golden question set and measure retrieval hit rate and answer faithfulness.
- Add hybrid retrieval (keyword plus vector) and a reranker.

## Skills demonstrated

RAG pipeline design, prompt engineering, LLM API integration, hallucination control through grounding and refusal, knowledge retrieval.
