# Hybrid Chunking

LongParser's `HybridChunker` combines **6 strategies** to produce RAG-optimized chunks that respect document structure.

## The 6 Strategies

| # | Strategy | What it does |
|---|---|---|
| 0 | **Equation detection** | Pre-pass re-tags missed formula blocks |
| 1 | **Structure-aware** | Groups blocks by hierarchy path (sections) |
| 2 | **Layout-aware** | Classifies blocks by type before chunking |
| 3 | **Token-window packing** | Packs blocks up to `max_tokens` with overlap |
| 4 | **Table-aware** | Schema chunk + row-batched table chunks |
| 5 | **List-aware** | Keeps bullet groups together as single chunks |

## Usage

```python
from longparser.chunkers import HybridChunker
from longparser.schemas import ChunkingConfig

config = ChunkingConfig(
    max_tokens=512,
    overlap_tokens=64,
    detect_equations=True,
    table_chunk_format="row_record",  # or "pipe"
    generate_schema_chunks=True,
)

chunker = HybridChunker(config)
chunks = chunker.chunk(doc.blocks)
```

## Table Chunking

Tables are chunked with:
- **Schema chunk** — column names, types, null rates, sample rows
- **Data chunks** — row-batched with header repeated per chunk
- **Wide-table banding** — columns split into bands for very wide tables

```python
# Table chunk formats
ChunkingConfig(table_chunk_format="row_record")
# Row 1: col_a=val; col_b=val; col_c=val

ChunkingConfig(table_chunk_format="pipe")
# col_a | col_b | col_c
# val_1 | val_2 | val_3
```

## Chunk Structure

```python
@dataclass
class Chunk:
    chunk_id: str           # UUID
    text: str               # Chunk content
    token_count: int        # Approximate token count
    chunk_type: str         # section | table | table_schema | list | equation
    section_path: list[str] # ["Introduction", "Methods"]
    page_numbers: list[int] # [1, 2]
    block_ids: list[str]    # Source block IDs for traceability
    metadata: dict          # chunk_type-specific metadata
```

## Token Budget

Chunks respect a hard `max_tokens` ceiling. Equations are kept with their surrounding context using a **glue heuristic**:

- If the *next* block is an equation AND the current window overflows, the last paragraph carries over into the new chunk (so the equation is never split from its context).
