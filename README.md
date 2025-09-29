# Breast Cancer Receptor Status Extraction

A Python package for extracting ER/PR/HER2 status from electronic health records using natural language processing and structured data extraction.

## Installation

```bash
pip install bc-receptor-extraction
```

## Quick Start

```python
from bc_receptor import ReceptorExtractor

extractor = ReceptorExtractor()
text = "Pathology: ER positive (85%), PR positive (70%), HER2 3+"
results = extractor.extract_from_text(text)
print(results)
```

## Status

Under active development

## License

MIT
