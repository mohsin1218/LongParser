# Installation

## Requirements

- Python 3.10, 3.11, or 3.12
- Tesseract OCR (`brew install tesseract` / `apt install tesseract-ocr`)

## Core Install

```bash
pip install longparser
```

## Extras

LongParser uses optional extras to keep the base install lightweight:

| Extra | Installs |
|---|---|
| `server` | FastAPI, LangChain, LangGraph, MongoDB motor |
| `langchain` | LangChain core + community |
| `llamaindex` | LlamaIndex core + readers |
| `ocr` | pix2tex (LaTeX OCR) |

```bash
# Full server stack
pip install "longparser[server]"

# Development
pip install "longparser[server,langchain,llamaindex,dev]"
```

## With uv (recommended for development)

```bash
git clone https://github.com/ENDEVSOLS/LongParser.git
cd LongParser
uv sync --extra server
```

## Docker

```bash
docker compose up
```

The server starts on `http://localhost:8000`.

## Verify Installation

```python
import longparser
print(longparser.__version__)  # 0.1.0
```
