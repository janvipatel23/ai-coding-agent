# 🤖 AI Coding Agent

A lightweight **command-line coding assistant** powered by **Google Gemini 2.5 Flash**. Ask any coding question, request code generation, debug an error, or get a technical explanation — directly from your terminal with a single command.

---

## ✨ Features

- **Gemini 2.5 Flash** — Google's latest fast, high-quality reasoning model via the official `google-genai` SDK.
- **CLI-First** — Zero UI overhead. Pass your prompt as a command-line argument and get an instant response.
- **Single-File Architecture** — The entire agent is 24 lines of Python in one file.
- **Minimal Dependencies** — Only two runtime packages: `google-genai` and `python-dotenv`.
- **Secure API Key Handling** — API key loaded from `.env` via dotenv, never hardcoded.

---

## 🏗️ Architecture

```
$ python main.py "Your coding question here"
          │
          ▼
Load GEMINI_API_KEY from .env
          │
          ▼
genai.Client(api_key=api_key)
          │
          ▼
types.Content(
  role="user",
  parts=[types.Part(text=user_prompt)]
)
          │
          ▼
client.models.generate_content(
  model="gemini-2.5-flash",
  contents=messages
)
          │
          ▼
print(response.text)  →  Terminal output
```

---

## 🛠️ Tech Stack

| Layer | Technology | Version | Purpose |
|---|---|---|---|
| **Language** | Python | 3.12+ | Core application language |
| **LLM** | Google Gemini 2.5 Flash | — | Fast, high-quality AI responses |
| **AI SDK** | `google-genai` | 1.12.1 | Official Google Generative AI Python client |
| **Config** | `python-dotenv` | 1.1.0 | Load `GEMINI_API_KEY` from `.env` |
| **CLI** | Python `argparse` | stdlib | Parse `user_prompt` from command line |

---

## 📁 Project Structure

```
ai-coding-agent/
├── main.py           # Full agent — Gemini client setup, CLI arg parsing, response output
├── pyproject.toml    # Project config and pinned dependencies
├── .gitignore        # Excludes .env and other sensitive files
├── LICENSE           # Apache-2.0
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.12+
- A [Google AI Studio API key](https://aistudio.google.com/app/apikey) (free tier available)

### 1. Clone the Repository

```bash
git clone https://github.com/janvipatel23/ai-coding-agent.git
cd ai-coding-agent
```

### 2. Install Dependencies

```bash
pip install google-genai==1.12.1 python-dotenv==1.1.0
```

Or using `pyproject.toml`:
```bash
pip install .
```

### 3. Configure Your API Key

Create a `.env` file in the project root:

```env
GEMINI_API_KEY=your_google_gemini_api_key_here
```

> Get your free API key at [aistudio.google.com](https://aistudio.google.com/app/apikey).

### 4. Run the Agent

```bash
python main.py "Your coding question here"
```

---

## 💬 Example Usage

**Generate code:**
```bash
python main.py "Write a Python function to reverse a linked list"
```

**Debug an error:**
```bash
python main.py "Why does this Python code throw a TypeError: 'NoneType' object is not iterable?"
```

**Explain a concept:**
```bash
python main.py "Explain the difference between RAG and fine-tuning in simple terms"
```

**SQL query help:**
```bash
python main.py "Write a SQL query to find the top 5 customers by total spend"
```

**Code review:**
```bash
python main.py "Review this code for potential bugs: def divide(a, b): return a/b"
```

---

## 🔍 How It Works

The agent constructs a single-turn conversation using the Gemini SDK's typed message format:

```python
# Build a typed user message
messages = [
    types.Content(
        role="user",
        parts=[types.Part(text=args.user_prompt)]
    )
]

# Send to Gemini 2.5 Flash and print the response
response = client.models.generate_content(
    model="gemini-2.5-flash",
    contents=messages
)

print(response.text)
```

The `types.Content` and `types.Part` structure comes from the official `google.genai.types` module — providing type safety and compatibility with the full Gemini API feature set (multimodal, system instructions, streaming, etc.) as the agent evolves.

---

## ⚙️ Configuration Reference

| Parameter | Value | Description |
|---|---|---|
| `model` | `gemini-2.5-flash` | Gemini model used for generation |
| `role` | `user` | Message role for the user turn |
| `GEMINI_API_KEY` | from `.env` | Google AI Studio API key |
| `requires-python` | `>=3.12` | Minimum Python version |

---

## 🔀 Switching Models

To use a different Gemini model, update the `model` parameter in `main.py`:

```python
# Fast and efficient (current)
model='gemini-2.5-flash'

# Most capable
model='gemini-2.5-pro'

# Previous generation
model='gemini-1.5-flash'
model='gemini-1.5-pro'
```

Browse all available models at [Google AI Studio](https://aistudio.google.com/).

---

## 📦 Dependencies

```toml
dependencies = [
    "google-genai==1.12.1",
    "python-dotenv==1.1.0",
]
```

Install:
```bash
pip install google-genai==1.12.1 python-dotenv==1.1.0
```

---

## 🔮 Extending the Agent

The current implementation is a solid foundation for extending into a full agentic workflow:

**Add conversation history** — maintain a `messages` list across turns for multi-turn dialogue.

**Add system instructions** — prepend a `system` role message to give the agent a persona or coding style.

**Add tool use** — connect Gemini's function calling to tools like code execution, file I/O, or web search.

**Add streaming** — use `client.models.generate_content_stream()` for token-by-token output.

```python
# Streaming example
for chunk in client.models.generate_content_stream(
    model="gemini-2.5-flash",
    contents=messages
):
    print(chunk.text, end="", flush=True)
```

---

## 🤝 Contributing

Contributions, bug reports, and feature requests are welcome! Feel free to open a pull request or issue on [GitHub](https://github.com/janvipatel23/ai-coding-agent).

---

## 📄 License

This project is licensed under the **Apache-2.0 License**. See the [LICENSE](./LICENSE) file for details.