# 🇮🇳 Yojana Saathi

An AI assistant that helps Indian citizens — especially rural users — discover the government schemes they're actually eligible for. Understands Hindi, Hinglish, and English, with voice input/output, built entirely on free-tier tools.

[![Python](https://img.shields.io/badge/Python-3.14-blue)](#)
[![FastAPI](https://img.shields.io/badge/FastAPI-backend-009688)](#)
[![ChromaDB](https://img.shields.io/badge/ChromaDB-vector%20search-orange)](#)
[![Gemini](https://img.shields.io/badge/Gemini-API-8E75B2)](#)
[![License](https://img.shields.io/badge/license-MIT-green)](#)

---

## ✨ What it does

| Feature | How |
|---|---|
| **Understands Hindi / Hinglish / English** | User's question is rewritten into clear Hindi/English before search, since Romanized Hinglish embeds poorly |
| **Finds relevant schemes** | Semantic search over a local vector database of government schemes — no exact keyword match needed |
| **Checks eligibility** | Builds up a user profile (age, income, land, category, etc.) conversationally and matches it against each scheme's real rules |
| **Voice in, voice out** | Browser's built-in speech recognition and text-to-speech — no paid voice API |
| **Works on low-end phones** | Frontend is plain HTML + Tailwind + vanilla JS, no heavy framework, no build step |

---

## 🛠 How it's built

- **Backend** — Python 3.14, FastAPI
- **Vector search (RAG)** — ChromaDB (local, persistent) + a multilingual sentence-transformers embedding model, so Hindi/Hinglish/English queries all match the same scheme data
- **LLM** — Google Gemini API (free tier, `gemini-3.5-flash-lite`) for understanding the question and generating the reply, with real API calls budgeted and rate-limited
- **Frontend** — static HTML/CSS/JS with the Web Speech API for voice, built for slow networks and low-end devices
- **Deployment target** — Hugging Face Spaces (Docker)

---

## 📁 Project structure

```
.
├── backend/
│   ├── app/          # FastAPI entrypoint, routes, config
│   ├── services/      # Retrieval, embeddings, Gemini calls, eligibility logic
│   ├── models/         # Pydantic request/response + scheme schemas
│   ├── prompts/        # System prompts for Gemini
│   └── tests/           # pytest tests
├── data/
│   └── schemes.json    # Government scheme dataset
├── frontend/           # Static HTML/CSS/JS served to the browser
├── scripts/            # Ingest data, search test, CLI chat, e2e conversation test
├── SETUP.md            # Full setup guide
├── DATA_GUIDE.md        # How scheme data is structured
└── Dockerfile
```

---

## 🚀 Run it locally

```bash
git clone https://github.com/Krish-Khati/yojana-saathi.git
cd yojana-saathi
python -m venv venv
venv\Scripts\activate      # Windows — use `source venv/bin/activate` on macOS/Linux
pip install -r requirements.txt
copy .env.example .env     # add your free Gemini API key
```

Build the search index and start the server:

```bash
python scripts/ingest.py
uvicorn backend.app.main:app --reload
```

Other useful commands:

```bash
pytest                            # run tests
python scripts/chat_cli.py        # chat with the assistant in the terminal
python scripts/test_search.py     # check search results for sample queries
```

Full step-by-step setup (Python version, venv, env vars) is in [`SETUP.md`](SETUP.md).

---

## 📄 License

MIT