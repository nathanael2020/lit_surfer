# lit_surfer.py

## Overview
`lit_surfer.py` is a Python script designed for academic research automation. It searches, downloads, summarizes, and emails summaries of academic papers from arXiv. The script uses OpenAI's API python package `openai` to connect with the open source `Mixtral-8x7B-Instruct-v0.1` for generating summaries and critiques.

By default, the script uses together.ai. Optionally, you can uncomment the openai api url and model to use GPT-4 (or another model).

## Features
- **Automated arXiv Paper Search**: Search for papers on arXiv using user-defined terms.
- **PDF Management**: Download and manage PDFs of academic papers.
- **AI Summarization**: Summarize and critique academic papers using OpenAI, Together.ai, or other GPT models.
- **Email Integration**: Email summaries and critiques of papers.

## Requirements
- Python 3.x (I used 3.11.6)
- Packages: `requests`, `xml.etree.ElementTree`, `json`, `os`, `subprocess`, `shutil`, `smtplib`, `argparse`, `PyPDF2`, `dotenv`, `openai`.
- API Keys (for together.ai and openai.com; read more here: https://docs.together.ai/docs/get-started and https://help.openai.com/en/articles/4936850-where-do-i-find-my-api-key).
- Google application password to support emailing the summaries (read more here: https://support.google.com/accounts/answer/185833).

## Installation
1. Clone the repository:
```
git clone https://github.com/nathanael2020/lit_surfer.git
```
2. Install dependencies:
```
pip install -r requirements.txt
```
3. Configure `.env` file for API keys and email credentials.

## Usage
Run the script with a search term, a starting number, and the number of results:
```
python lit_surfer.py 'biological+AND+neural+AND+agent' 0 10
```
Here:
```
search_term = ''biological+AND+neural+AND+agent'
start = 0
max_results = 10
```
## Optional Usage
If you don't want to use the email feature, leave the google password out of your .env file. The results of your query will be saved to a json file in the lit_surfer directory.

```
# biological+AND+neural+AND+agent_10_processed_api_data.json

{
    "2312.17179v1": {
        "title": [Going Green in RAN Slicing]],
        "authors": [
            "Hnin Pann Phyu",
            "Razvan Stanica",
            "Diala Naboulsi",
            "Gwenael Poitau"
        ],
        "abstract": [],
        "published_date": []],
        "scraped_time": []],
        "pdf_url": "http://arxiv.org/pdf/2312.17179v1",
        "body": [],
        "gpt_response": []
         "tldr": []
    }
}

```
## Configuration
- Set API keys and email credentials in `.env` file.
```
# .env

OPEN_AI_API_KEY=[YOUR_OPENAI_API_KEY]
TOGETHER_API_KEY=[YOUR_TOGETHER_API_KEY]
GOOGLE_PASSWORD=[YOUR_GOOGLE_PASSWORD]
RECEIVER_EMAIL=[user@example.com]
SENDER_EMAIL=[user_example.com]
FILE_PREFIX='/path/to/lit_surfer'
```

By default, the script uses together.ai. Optionally, you can uncomment the openai api url and model to use GPT-4 (or another model).

- Customize search terms in the command line arguments and the number of papers to process.

## LLM Summarization
Each paper is summarized in five steps:
1. The first 2,000 and last 2,000 characters (before the References section) along with the abstract are passed to the LLM:

- memory = content + abstract

`f"{memory}\n\nSummarize this material in no more than six paragraphs, first two paragraphs summarizing the research and results, then two more paragraphs describe how this research fits into the existing body of research. Then include two more paragraphs critiquing the research. Before responding, rewrite it at least 3 times, improving it each time. Think deeply on the research context, the achievements of this research, what the authors' peers would find lacking, and what a skeptical critic would admit is valuable. Respond with the best final draft of your six paragraph summary. Preface your response with 'Draft Summary:\n\n'"`

2. The first 2,000 and last 2,000 characters (before the References section) along with the abstract and the six-paragraph summary are passed to the LLM:

- memory = content + abstract + first summary

`f"{memory}\n\nReview the original paper and evaluate the quality of the summary. Identify 5 points of weakness that need improvement. Respond with details on how to improve the summary. Don't resummarize yourself. Preface your response with 'Critique:\n\n'"`

3. The first 2,000 and last 2,000 characters (before the References section) along with the abstract, the first six-paragraph summary, and the critique are passed to the LLM:

- memory = content + abstract + first summary + critique

`{memory}\n\nReview the original paper, the draft summary, and the critique (points of needed improvement). Rewrite the summary in exactly six paragraphs considering these points. Then rewrite it again 4 times, improving it each time, and really considering the paper with a skeptical eye. Be sure to check all your facts by thoroughly reviewing the paper. Respond only with your best final draft of the six paragraphs, not the first three revisions. Preface your response with 'Draft Summary:\n\n'"`

4. The first 2,000 and last 2,000 characters (before the References section) along latest draft six-paragraph summary are passed to the LLM:

- memory = content + draft summary

`{memory}\nReview the paper and the draft summary and refine the summary further, condense it to exactly four very well-written paragraphs. Be sure to check all your facts by thoroughly reviewing the paper and rewriting a final time. Respond only with your four-paragraph final summary.`

5. The first 2,000 and last 2,000 characters (before the References section) along with the final six-paragraph summary are passed to the LLM:

- memory = content + final summary

`{memory}\nReview the paper and the draft summary and refine the summary further. Be sure to check all your facts by thoroughly reviewing the paper. Reply with a three sentence 'tl;dr' summary explaining where this fits into the broader field of research and why it's important, as if you're talking to a general audience. No more than three sentences, and no more than one paragraph.`

# Future Improvements:
- For long papers, the middle sections could be separately read and summarized by the LLM, rather than only excerpting the beginning and end by character count.
- For papers with meaningful graphical and tabular content that isn't sufficiently summarized in the original paper, use [appjsonify](https://github.com/hitachi-nlp/appjsonify) to extract all material and automatically parse and review the content there using an image recognition and analysis program. [appjsonify](https://github.com/hitachi-nlp/appjsonify) works really well, but requires `detectron2` and `PyTorch` and can be finicky about versioning, especially for GPU use.

## Contribution
Contributions are welcome. Fork the repository and submit pull requests.

## License
MIT Open License

## Contact
nathanael@astrowaffle.anonaddy.com