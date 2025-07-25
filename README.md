# Cloudwalk Documentation Crawler and RAG Agent

An intelligent documentation crawler and RAG (Retrieval-Augmented Generation) agent built using Pydantic AI and Supabase. The agent can crawl websites, store content in a vector database, and provide intelligent answers to user questions by retrieving and analyzing relevant documentation chunks.

## Features

- Website crawling and chunking
- Vector database storage with Supabase
- Semantic search using OpenAI embeddings
- RAG-based question answering
- Support for code block preservation
- Streamlit UI for interactive querying
- Available as both API endpoint and web interface

## Prerequisites

- Python 3.11+
- Supabase account and database
- OpenAI API key
- Streamlit (for web interface)

## Installation

1. Clone the repository:
```bash
git clone https://github.com/strangercacaus/nimbus-challenge
```

2. Install dependencies (recommended to use a Python virtual environment):
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
```

3. Set up environment variables:
   - Rename `.env.example` to `.env`
   - Edit `.env` with your API keys and preferences:
   ```env
   OPENAI_API_KEY=your_openai_api_key
   SUPABASE_URL=your_supabase_url
   SUPABASE_SERVICE_KEY=your_supabase_service_key
   SITEMAP_URL =your_sitemap_url
   SITE_URL=your_website_url
   LLM_MODEL= 'gpt-4o'  # or your preferred OpenAI model
   EMBEDDING_MODEL= 'text-embedding-3-large' # or your Preferred Embedding Model
   ```

## Usage

### Database Setup

Execute the SQL commands in `site_pages.sql` to:
1. Create the necessary tables
2. Enable vector similarity search
3. Set up Row Level Security policies

In Supabase, do this by going to the "SQL Editor" tab and pasting in the SQL into the editor there. Then click "Run".

### Crawl Documentation

To crawl and store documentation in the vector database:

```bash
python crawler.py
```

This will:
1. Fetch URLs from the documentation sitemap
2. Crawl each page and split into chunks
3. Generate embeddings and store in Supabase

### Streamlit Web Interface

For an interactive web interface to query the documentation:

```bash
streamlit run streamlit_ui.py
```

The interface will be available at `http://localhost:8501`

## Configuration

### Database Schema

The Supabase database uses the following schema:
```sql
CREATE TABLE site_pages (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    url TEXT,
    chunk_number INTEGER,
    title TEXT,
    summary TEXT,
    content TEXT,
    metadata JSONB,
    embedding VECTOR(3072)
);
```

### Chunking Configuration

You can configure chunking parameters in `crawler.py`:
```python
chunk_size = 5000  # Characters per chunk
```

The chunker intelligently preserves:
- Code blocks
- Paragraph boundaries
- Sentence boundaries

## Project Structure

- `crawl_docs.py`: Documentation crawler and processor
- `cloudwalk_ai_expert.py`: RAG agent implementation
- `streamlit_ui.py`: Web interface
- `site_pages.sql`: Database setup commands
- `requirements.txt`: Project dependencies

## Error Handling

The system includes robust error handling for:
- Network failures during crawling
- API rate limits
- Database connection issues
- Embedding generation errors
- Invalid URLs or content

## Preview

Here are some screenshots of the CloudWalk AI Expert in action:

### Interface Screenshots

![Screenshot 1](https://raw.githubusercontent.com/strangercacaus/nimbus-challenge/main/assets/Screenshot%202025-07-13%20at%2019.08.27.png)
*CloudWalk Expert interface showing the main chat with an english resposne*

![Screenshot 2](https://raw.githubusercontent.com/strangercacaus/nimbus-challenge/main/assets/Screenshot%202025-07-13%20at%2019.08.37.png)
*AI agent responding to queries about CloudWalk's Products*

![Screenshot 3](https://raw.githubusercontent.com/strangercacaus/nimbus-challenge/main/assets/Screenshot%202025-07-13%20at%2019.08.43.png)
*Detailed conversation showing Multi-Language support for RAG retrieval in action*

![Screenshot 4](https://raw.githubusercontent.com/strangercacaus/nimbus-challenge/main/assets/Screenshot%202025-07-13%20at%2019.10.45.png)
*Example of comprehensive answers with source citations*

![Screenshot 5](https://raw.githubusercontent.com/strangercacaus/nimbus-challenge/main/assets/Screenshot%202025-07-13%20at%2019.10.51.png)
*CloudWalk Expert providing the user with code snippets for Stratus Smart Contract Implemmentation*

The interface provides an intuitive way to interact with the CloudWalk AI Expert, featuring:
- Clean, modern design with dark theme support
- Real-time streaming responses
- Source citations for transparency
- Interactive chat interface
- Proper handling of markdown formatting and code blocks
