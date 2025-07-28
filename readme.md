# PDF Processing Challenges - 1a & 1b
This repository contains solutions for two PDF processing challenges:
- **Challenge 1a**: Extract document title and headings from PDFs and output them as structured JSON.
- **Challenge 1b**: Extract text sections from PDFs, rank them by relevance to a given persona and job description using TF‑IDF and cosine similarity, and output the top results.
---
## **Repository Structure**
PDF-Processing-Challenges/
├── Challenge_1a/                      # Challenge 1a - Extract title & headings
│   ├── input/                         # Folder for input PDFs (mounted at runtime)
│   ├── output/                        # Folder for generated JSON output (mounted at runtime)
│   ├── process_pdfs.py                # PDF title & heading extraction script
│   ├── Dockerfile                     # Dockerfile for Challenge 1a
│   └── README.md                      # Challenge-specific instructions
│
├── Challenge_1b/                      # Challenge 1b - Rank PDF sections by persona/job
│   ├── input/                         # Folder for input PDFs (mounted at runtime)
│   ├── output/                        # Folder for generated JSON output (mounted at runtime)
│   ├── process_pdfs.py                # PDF section ranking script (TF-IDF + cosine similarity)
│   ├── Dockerfile                     # Dockerfile for Challenge 1b
│   └── README.md                      # Challenge-specific instructions
│
├── .gitignore                         # Ignore Python cache, build artifacts, etc.
├── LICENSE                            # Optional license (MIT, Apache, etc.)
├── README.md                          # **Universal readme for both projects (main readme)**
└── requirements.txt                   # (Optional) Combined dependencies for local use
---
## **Technologies Used**
- **Python 3.9+**
- [PyMuPDF (fitz)](https://pymupdf.readthedocs.io/) – PDF parsing
- [scikit-learn](https://scikit-learn.org/stable/) – TF-IDF & cosine similarity (Challenge 1b)
- [NumPy](https://numpy.org/) – numerical operations
- [JSON](https://docs.python.org/3/library/json.html) – structured output
---
## **Pre-requisites**
- [Docker](https://docs.docker.com/get-docker/) installed.
- PDFs placed in the `input/` folder before running.
---
## **Usage**
### **1. Challenge 1a**
#### **Build**
```bash
cd Challenge_1a
docker build -t challenge_1a-solution:latest .

Run
docker run --rm \-v $(pwd)/input:/app/input \-v $(pwd)/output:/app/output \challenge_1a-solution:latest

•	Input: PDF files inside input/.
•	Output: JSON files with extracted title & headings inside output/.
⸻
2. Challenge 1b

Build
cd Challenge_1b
docker build -t challenge_1b-solution:latest .

Run
docker run --rm \-v $(pwd)/input:/app/input \-v $(pwd)/output:/app/output \challenge_1b-solution:latest

•	Input: PDF files inside input/.
•	Output: persona_output.json containing top‑ranked sections inside output/.
⸻
Example Workflow

# Place PDF file
cp mydoc.pdf Challenge_1a/input/

# Run Challenge 1a
cd Challenge_1a
docker run --rm -v $(pwd)/input:/app/input -v $(pwd)/output:/app/output challenge_1a-solution:latest

# Check output
cat output/mydoc.json

# Run Challenge 1b with same file
cd ../Challenge_1b
docker run --rm -v $(pwd)/input:/app/input -v $(pwd)/output:/app/output challenge_1b-solution:latest
cat output/persona_output.json

Output Formats

Challenge 1a
{
  "title": "Document Title",
  "outline": [
    {"level": "H1", "text": "Heading 1", "page": 1},
    {"level": "H2", "text": "Heading 2", "page": 2}
  ]
}

Challenge 1b
{
  "metadata": {
    "persona": "PhD Researcher in Computational Biology",
    "job_to_be_done": "Prepare literature review on GNNs",
    "timestamp": "Thu Jul 25 17:03:54 2025"
  },
  "sections": [
    {
      "document": "sample.pdf",
      "page": 1,
      "section_title": "Introduction",
      "content": "Full page text here...",
      "importance_rank": 0.87
    }
  ]
}

.....