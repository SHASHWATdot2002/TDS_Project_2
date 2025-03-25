# FastAPI with Ollama Integration

This project provides a FastAPI-based web service that combines file processing (ZIP/CSV) capabilities with LLM-powered question answering using Ollama.

## Features

- Process ZIP files containing CSV data
- Extract answers from CSV files
- Integration with Ollama LLM for question answering
- Docker support for easy deployment
- GPU acceleration support (if available)

## Prerequisites

- Docker and Docker Compose
- Or for local development:
  - Python 3.11+
  - Ollama installed locally

## Quick Start with Docker

1. Clone the repository:
```bash
git clone <repository-url>
cd <repository-name>
```

2. Start the services:
```bash
docker-compose up -d
```

3. Pull the LLama2 model:
```bash
docker-compose exec ollama ollama pull llama2
```

4. The API will be available at `http://localhost:8000`

## Local Development Setup

1. Install dependencies:
```bash
pip install -r requirements.txt
```

2. Install and start Ollama:
```bash
# Install Ollama from https://ollama.ai
ollama serve
```

3. Pull the LLama2 model:
```bash
ollama pull llama2
```

4. Start the FastAPI application:
```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

## API Usage

### POST /api/
Process a question with optional file input.

**Parameters:**
- `question` (required): The question to be answered
- `file` (optional): ZIP file containing a CSV with an 'answer' column

**Example without file:**
```bash
curl -X POST "http://localhost:8000/api/" \
  -H "Content-Type: multipart/form-data" \
  -F "question=How many Wednesdays are there in the date range 1989-02-14 to 2007-09-09?"
```

**Example with file:**
```bash
curl -X POST "http://localhost:8000/api/" \
  -H "Content-Type: multipart/form-data" \
  -F "question=What is the answer in the CSV file?" \
  -F "file=@path/to/your/file.zip"
```

**Response format:**
```json
{
  "extracted_answer": "Value from CSV if file provided",
  "llm_answer": "Response from Ollama LLM",
  "question": "Original question asked"
}
```

## Error Handling

The API provides clear error messages for common issues:
- Missing or invalid ZIP/CSV files
- Missing 'answer' column in CSV
- Ollama service not running
- LLama2 model not installed
- Connection timeouts

## Environment Variables

- `OLLAMA_API_URL`: URL for the Ollama API (default: "http://127.0.0.1:11434/api/generate")

## Project Structure

```
.
├── main.py              # FastAPI application
├── requirements.txt     # Python dependencies
├── Dockerfile          # Container definition
├── docker-compose.yml  # Docker services configuration
└── README.md          # This file
```

## Technical Details

- FastAPI for the web framework
- Pandas for CSV processing
- Ollama for LLM integration
- Docker for containerization
- Support for GPU acceleration

## License

[Your chosen license]

## Contributing

[Your contribution guidelines]