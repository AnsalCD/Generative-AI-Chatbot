# Generative AI Chatbot

A simple, streaming-free chat interface built with **Streamlit** and **Groq**, using LangChain's `ChatGroq` wrapper for fast LLM responses. Maintains full conversation history within a session so the assistant has context across turns.

## Overview

This is a minimal chatbot app: the user types a question, it's appended to the session's chat history, the full history is sent to a Groq-hosted LLM, and the assistant's reply is displayed and saved back into history.

### Conversation Flow

```
User submits query
      ↓
Query displayed in chat UI
      ↓
Query appended to chat_history
      ↓
Full chat_history sent to LLM (with a system prompt prepended)
      ↓
LLM response received
      ↓
Response appended to chat_history
      ↓
Response displayed in chat UI
```

Each request to the LLM includes the full conversation so far, e.g.:
```json
[
  {"role": "system", "content": "You are a helpful assistant"},
  {"role": "user", "content": "What is ML?"}
]
```

## Tech Stack

- **[Streamlit](https://streamlit.io/)** — chat UI (`st.chat_message`, `st.chat_input`, session state)
- **[LangChain](https://www.langchain.com/)** (`langchain-groq`, `langchain-community`) — LLM integration
- **[Groq](https://groq.com/)** — fast LLM inference backend
- **python-dotenv** — environment variable management

## Project Structure

```
.
├── chatbot.py          # Main Streamlit app
├── requirements.txt
└── .env                # Your local environment file (not committed — see below)
```

## Setup

### Prerequisites

- Python 3.11+
- A [Groq API key](https://console.groq.com/keys)

### Installation

```bash
# Clone the repo
git clone <your-repo-url>
cd <your-repo-name>

# Create and activate a virtual environment
python3 -m venv venv
source venv/bin/activate   # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### Environment Variables

Copy the provided template and fill in your own key:

```bash
cp env_template.txt .env
```

Then edit `.env` to set your actual key:

```
GROQ_API_KEY="your-api-key"
```

> Make sure `.env` is listed in your `.gitignore` so it's never committed. Only `env_template.txt` (with a placeholder) should be tracked in the repo.

## Usage

Run the app with:

```bash
streamlit run chatbot.py
```

This opens the chatbot in your browser (typically at `http://localhost:8501`). Type a question in the input box at the bottom — the assistant will respond, and the full conversation persists for the duration of your browser session.

## Configuration

| Parameter | Location | Description |
|---|---|---|
| `model` | `chatbot.py` | Groq model used for generation (currently `openai/gpt-oss-20b`) |
| `temperature` | `chatbot.py` | LLM sampling temperature (default: 0.0 for deterministic answers) |
| System prompt | `chatbot.py` | Currently `"You are a helpful assistant"` — edit inline to change the assistant's persona |

## Notes

- Chat history is stored in `st.session_state` only, so it resets when the browser tab/session ends — there's no persistent storage across sessions.
- Groq's model lineup changes over time — if you hit a `model_decommissioned` error, check the [Groq models page](https://console.groq.com/docs/models) for the current recommended replacement.

## License

Add your license of choice here (e.g. MIT).
