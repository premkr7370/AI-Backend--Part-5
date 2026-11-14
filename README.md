# Document Q&A API

A Node.js and Express API that answers questions using retrieval-augmented generation (RAG). It searches a Qdrant vector collection for relevant document chunks and sends those chunks to a Groq chat model as context.

## Requirements

- Node.js (current LTS recommended)
- A Groq API key
- A Google AI API key for Gemini embeddings
- A Qdrant instance with an existing `nodejsDocumentation` collection containing embeddings generated with `gemini-embedding-001`

## Setup

1. Install dependencies:

   ```bash
   npm install
   ```

2. Create a `.env` file in this directory:

   ```env
   GROQ_API_KEY=your_groq_api_key
   GOOGLE_API_KEY=your_google_api_key
   QDRANT_URL=https://your-qdrant-instance
   QDRANT_API_KEY=your_qdrant_api_key
   PORT=5000
   ```

   `PORT` is optional and defaults to `5000`. Keep API keys private; do not commit `.env`.

3. Start the server:

   ```bash
   npm run dev
   ```

The server connects to Qdrant during startup, so the configured URL, credentials, and collection must be available before it starts.

## API

### `POST /chat`

Send a JSON body containing a question:

```json
{
  "prompt": "What does the document say about ...?"
}
```

The API retrieves the five most similar chunks from the `nodejsDocumentation` collection, passes them with the prompt to the chat model, and returns the model response content as JSON.

## Document data

The repository includes `knowledge.pdf`. The source defines a helper that reads and splits this PDF into chunks and adds them to Qdrant, but that helper is not called when the server starts. As written, the API expects the Qdrant collection to have been populated separately before use.
