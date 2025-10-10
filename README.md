# capstone
## Project Overview

For the final capstone implementation, I have stuck to the AI-based model as my main extraction pipeline.
The earlier version, which relied purely on regex and keyword matching, was limited in accuracy and could only detect a few pre-defined geological terms. It also struggled with variations in sentence structure and contextual references.
Hence, this final model focuses on page-aware, schema-guided extraction using a lightweight GPT-4o-mini agent, ensuring higher contextual precision and structured output across all geology-related entities.

## Model Workflow Summary
## Input and Setup
The model runs in Google Colab with dependencies installed via pip install openai pdfplumber pandas.
The OpenAI API key is securely loaded from userdata.get("sandra").
A single research thesis PDF is provided as the input (e.g., 2007_Tshibubudze_THE MARKOYE FAULT_2007.pdf).

## Page-Aware Text Extraction
Each page is parsed using pdfplumber, extracting text while preserving per-page boundaries.
Basic cleaning removes extra newlines, page breaks, and hyphenated word splits.
The script automatically excludes sections like “Abstract”, “Acknowledgements”, and “References”, focusing only on core thesis content.

## AI-Driven Information Extraction
For every page, the text (up to ~5500 chars) is sent to the GPT-4o-mini model with a structured JSON schema covering:
Metadata (title, author, institution, etc.)
Geology (region, formations, rock types, minerals, tectonic settings)
Geochronology (sample IDs, ages, methods, evidence)
Geochemistry (analytes, units, values, methods, context)
Metallogeny (ore types, host rocks, alteration features)
The model acts as a geology aware data extractor, returning pure JSON responses without any explanations.
Each record includes the originating page number, enabling traceability and page level validation.

## JSON Consolidation and Reference Flattening
All page-level outputs are combined into a single extracted.json file.
A flattening step normalizes nested lists and dictionary entries into a structured table.
Duplicate entities from multiple pages are merged while keeping page references, producing extracted_flat_ref.csv.

## Output
Final deliverables:
extracted.json → full structured page wise JSON output.
extracted_flat_ref.csv → flattened, reference aware CSV ready for geodatabase import.
Each row maps a geological entity to its relevant section, field, and all page references.

# Key Features and Advantages
Page-Referenced AI Extraction — every entry maintains its original source location.
Schema-Guided Consistency — ensures uniform JSON structure across PDFs.
Contextual Understanding — captures implicit geological relationships missed by regex.
Cleaned Input Text — abstracts and references removed to improve prompt accuracy.
Reference - Aware CSV — merges repeated mentions into unified, verifiable records.

# Pipeline
PDF → Text Extraction → AI → JSON → CSV
