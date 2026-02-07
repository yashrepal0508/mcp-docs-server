# MCP Docs Server

A Model Context Protocol (MCP) server that provides tools for retrieving and cleaning official documentation from various AI libraries. This server allows AI agents to search and access up-to-date documentation to answer technical questions accurately.

## Features

- **Web Search Integration**: Uses Serper API to find relevant documentation pages.
- **Content Cleaning**: Extracts clean text from HTML using `trafilatura`, removing clutter.
- **Supported Libraries**:
  - LangChain (`langchain`)
  - LlamaIndex (`llama-index`)
  - OpenAI (`openai`)
  - UV (`uv`)
- **FastMCP**: Built using the `fastmcp` library for efficient MCP implementation.
- **Client Implementation**: Includes a sample `client.py` wrapper to demonstrate how to query the server and generate answers using a Groq LLM.

## Why use this? (Use Cases)

- **Overcoming Training Cutoffs**: LLMs often have training data cutoffs that predate the latest releases of fast-moving libraries like `langchain` or `uv`. This server provides *live* access to the latest documentation.
- **Reducing Hallucinations**: By retrieving the actual documentation before answering, the AI is grounded in reality and much less likely to invent non-existent API methods.
- **Agentic Workflows**: Perfect for autonomous coding agents that need to "read the manual" before attempting to use a complex tool or library.
- **Plug-and-Play**: As an MCP server, it can be easily integrated into any MCP-compliant client (like Claude Desktop or compatible IDEs) to instantly give them documentation search capabilities.

## Prerequisites

- Python 3.12+
- `uv` package manager (recommended)
- API Keys:
  - **Serper API Key**: For Google Search capabilities.
  - **Groq API Key**: For the client to generate answers (if using `client.py`).

## Installation

1. **Clone the repository:**
   ```bash
   git clone <repository_url>
   cd mcp-docs-server
   ```

2. **Install dependencies:**
   This project uses `uv` for dependency management.
   ```bash
   uv sync
   ```
   Or using pip:
   ```bash
   pip install -e .
   ```

## Configuration

Create a `.env` file in the root directory:

```bash
touch .env
```

Add your API keys to the `.env` file:

```env
SERPER_API_KEY=your_serper_api_key_here
GROQ_API_KEY=your_groq_api_key_here
```

> **Note**: The code currently expects the Groq API key to be stored in the `GROQ_API_KEY` environment variable.

## Usage

### Running the Server

The server is designed to be communicating over stdio. You can run it directly with:

```bash
uv run mcp_server.py
```

### Running the Client

The `client.py` script demonstrates how to connect to the MCP server programmatically. It:
1. Starts the MCP server as a subprocess.
2. Lists available tools.
3. Calls the `get_docs` tool with a query (default: "how to use chromadb with langchain?").
4. Uses an LLM (Llama 3.3 via Groq) to answer the question based on the retrieved context.

To run the client:

```bash
uv run client.py
```

## Project Structure

- `mcp_server.py`: Main MCP server implementation containing the `get_docs` tool.
- `client.py`: Example client that orchestrates the MCP server and an LLM.
- `utils.py`: Helper functions for HTML cleaning and LLM interaction.
- `pyproject.toml`: Project configuration and dependencies.

## Tool: `get_docs`

**Arguments:**
- `query`: The search query (e.g., "Publish a package with UV").
- `library`: The target documentation library (e.g., `uv`, `langchain`).

**Returns:**
- A string containing relevant snippets from the documentation with source links.
