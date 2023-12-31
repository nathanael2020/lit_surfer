# lit_surfer.py

## Overview
`lit_surfer.py` is a Python script designed for academic research automation. It searches, downloads, summarizes, and emails summaries of academic papers from arXiv. The script uses OpenAI's GPT models for generating summaries and critiques.

## Features
- **Automated arXiv Paper Search**: Search for papers on arXiv using user-defined terms.
- **PDF Management**: Download and manage PDFs of academic papers.
- **AI Summarization**: Summarize and critique academic papers using OpenAI GPT models.
- **Email Integration**: Email summaries and critiques of papers.

## Requirements
- Python 3.x (I used 3.11.6)
- Packages: `requests`, `xml.etree.ElementTree`, `json`, `os`, `subprocess`, `shutil`, `smtplib`, `argparse`, `PyPDF2`, `dotenv`, `openai`.

## Installation
1. Clone the repository:
```
git clone [repository URL]
```
2. Install dependencies:
```
pip install -r requirements.txt
```
3. Configure `.env` file for API keys and email credentials.

## Usage
Run the script with a search term, a starting number, and the number of results:
```
python lit_surfer.py 'biological+AND+neural+AND+agent 0 10
```
## Configuration
- Set API keys and email credentials in `.env` file.
- Customize search terms and the number of papers to process.

## Contribution
Contributions are welcome. Fork the repository and submit pull requests.

## License
MIT Open License