# LongParser

<p align="center">
  <strong>Privacy-first document intelligence engine for production RAG pipelines.</strong>
</p>

---

**LongParser** transforms raw PDFs, DOCX, PPTX, XLSX, and CSV files into validated, AI-ready chunks — with a built-in Human-in-the-Loop (HITL) review layer, 3-layer memory chat engine, and a production FastAPI server.

## Why LongParser?

Most RAG pipelines fail at the data layer. Hallucinations, missed tables, garbled equations, and unverified citations stem from poor document parsing — not from the LLM itself.

**LongParser solves the input problem.**

| Feature | LongParser |
|---|---|
| Multi-format extraction | PDF, DOCX, PPTX, XLSX, CSV |
| Hybrid chunking (6 strategies) | ✅ |
| HITL review workflow | ✅ |
| 3-layer memory chat | ✅ |
| Built-in citation validation | ✅ |
| LaTeX/equation parsing | ✅ |
| LangChain & LlamaIndex ready | ✅ |
| RTL language support | ✅ |
| Docker-ready server | ✅ |

---

## Quick Start

```bash
pip install longparser
```

```python
from longparser import DocumentPipeline, ProcessingConfig

pipeline = DocumentPipeline(ProcessingConfig())
doc = pipeline.process("report.pdf")

print(f"Extracted {len(doc.blocks)} blocks, {len(doc.chunks)} chunks")
```

---

## Architecture

```mermaid
graph LR
    A[Document] --> B[Extract]
    B --> C[Validate]
    C --> D[HITL Review]
    D --> E[Chunk]
    E --> F[Embed]
    F --> G[Index]
    G --> H[Chat Engine]
```

---

## Installation Options

=== "Core (PDF parsing only)"
    ```bash
    pip install longparser
    ```

=== "With REST Server"
    ```bash
    pip install "longparser[server]"
    ```

=== "With LangChain"
    ```bash
    pip install "longparser[langchain]"
    ```

=== "With LlamaIndex"
    ```bash
    pip install "longparser[llamaindex]"
    ```

=== "All extras"
    ```bash
    pip install "longparser[server,langchain,llamaindex]"
    ```

---

## Next Steps

- [**Installation Guide**](getting-started/installation.md) — detailed setup with virtual environments
- [**Quickstart**](getting-started/quickstart.md) — parse your first document in 5 minutes
- [**Configuration**](getting-started/configuration.md) — environment variables and tuning
