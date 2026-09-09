# Data governance architecture assistant - LLM + RAG A context-aware chatbot that answers questions about data governance architecture by retrieving relevant knowledge from a curated document corpus before generating responses with GPT-4o. 

## Architecture 
1. Documents ingested, chunked, and embedded into a vector store
2. User query embedded and matched against the corpus (semantic search)
3. Top-k chunks injected into GPT-4o prompt as grounded context
4. Response generated with citations and confidence indicators

## Key design decisions - Chunk size and overlap tuned for governance documentation specificity - System prompt enforces citation discipline and prevents hallucination - Retrieval evaluated using relevance scoring before generation 

## Tech stack Python · OpenAI GPT-4o API · LangChain / custom retrieval · Vector store · Jupyter Notebook 

## Skills demonstrated RAG pipeline design · Prompt engineering · LLM API integration · Responsible AI (hallucination control) · Knowledge retrieval
