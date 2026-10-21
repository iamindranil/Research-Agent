# Research Agent

A small command-line research assistant that searches the web, reads selected pages, and asks an AI model to write a structured report. It is built as a learning-friendly example of an AI agent that can use Python tools.

## How it works

The agent follows a loop:

1. You enter a research topic.
2. The AI chooses a next action: search, read a page, or write the report.
3. Python runs that action using the project's tools.
4. The tool result is sent back to the AI, which chooses what to do next.
5. When the AI writes the report, the program saves it as a Markdown file.

The AI chooses the action, but the Python program executes it.

## Project files

| File | Purpose |
| --- | --- |
| `01_search_tool.py` | Searches DuckDuckGo using `ddgs` and formats search results. |
| `02_scrape_tool.py` | Downloads a webpage and extracts readable text with Requests and BeautifulSoup. |
| `03_interactive_researcher.py` | Runs the interactive agent loop, dispatches tool calls, and saves reports. |
| `gemini_client.py` | Sends prompts to Gemini and can fall back to Groq or Cerebras when configured. |
| `requirements.txt` | Lists the Python package dependencies. |
| `.env.example` | Template for local API-key configuration. Copy it to `.env` and add your keys. |

## Requirements

- Python 3.10 or newer
- A Gemini API key
- Optional: Groq and/or Cerebras API keys for fallback providers
- Internet access for the AI provider, web search, and webpage fetching

## Setup

Open PowerShell in the project directory:

```powershell
cd D:\AI\Research-Agent
```

Create and activate a virtual environment (recommended):

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

If PowerShell does not recognize `python`, try the Windows Python launcher:

```powershell
py -3 -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Install the dependencies:

```powershell
python -m pip install -r requirements.txt
```

Create your local `.env` file from the template:

```powershell
Copy-Item .env.example .env
```

Edit `.env` and set `GEMINI_API_KEY` to your Gemini API key. Add `GROQ_API_KEY` and/or `CEREBRAS_API_KEY` if you want those providers available as fallbacks.

**Never commit `.env` or share its API keys.** The `.gitignore` file excludes local environment files, while allowing the safe `.env.example` template to be committed.

## Run the agent

```powershell
python .\03_interactive_researcher.py
```

Enter a specific topic, such as:

```text
How are heat pumps being used to reduce building emissions?
```

Type `quit`, `exit`, or `q` to stop.

The report is printed in the terminal and saved in the current working directory with a name like `research_report_20261021_194800.md`.

## Run individual tools

Search tool demo:

```powershell
python .\01_search_tool.py
```

Web scraper demo:

```powershell
python .\02_scrape_tool.py
```

## Configuration and behavior

- The Gemini client uses `GEMINI_API_KEY` and the model configured in `gemini_client.py`.
- Groq and Cerebras are optional fallbacks, configured with `GROQ_API_KEY` and `CEREBRAS_API_KEY`.
- The researcher allows at most 10 AI/tool steps by default.
- Each search action requests up to five results.
- The scraper returns up to 2,000 characters by default.
- Search results and page text are sent to the selected AI provider. Avoid researching private or sensitive information.
- Search providers, websites, and AI services can be temporarily unavailable, rate-limited, or return no results.

## Troubleshooting

### `ModuleNotFoundError`

Activate the virtual environment, then install the dependencies:

```powershell
python -m pip install -r requirements.txt
```

### API key missing

Check that `.env` exists in the project directory and contains a valid `GEMINI_API_KEY`. Restart the program after changing the file.

### Provider or search errors

Check your internet connection, API key validity, and provider availability. Configure a fallback provider if desired. An empty search response can also mean the search service returned no results for that query.

## Security

API keys are credentials. Keep them in the ignored `.env` file or in environment variables; do not place them in source code, README files, screenshots, or commits. If a key has been exposed, revoke it with its provider and replace it.
