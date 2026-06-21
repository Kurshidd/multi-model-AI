# 🤖 AI Assistant — Multi-Model AI System

A powerful AI assistant built with **Streamlit**, **LangGraph**, and **Groq**,
inspired by Perplexity AI and Tavily. Supports RAG over PDFs, live web search,
image understanding, and persistent chat history — all behind Google login.

---

## ✨ Features

| Feature | Details |
|---|---|
| 🔐 Google Login | OAuth 2.0 authentication — secure, no passwords |
| 💬 Smart Chat | Conversational AI powered by Llama 3.3 70B via Groq |
| 📄 RAG over PDF | Upload any PDF → ask questions → get answers from it |
| 🌐 Web Search | Live web results via Tavily with source citations |
| 🖼️ Image Understanding | Upload images → AI describes and answers questions |
| 🧠 LangGraph Routing | Automatically routes queries to the right agent |
| 💾 Chat History | All conversations saved per user in SQLite |
| 🗑️ Delete Chats | Delete individual sessions or clear all history |
| ⚡ Streaming | Token-by-token streaming with typing cursor animation |
| 🌙 Dark Theme | Clean dark UI styled like Perplexity / Tavily |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────┐
│           Streamlit Frontend            │
│  Chat UI · PDF Upload · Image Upload    │
│  Sidebar · Chat History · Google Login  │
└────────────────┬────────────────────────┘
                 │
┌────────────────▼────────────────────────┐
│         Google OAuth 2.0                │
│   Authentication · Session Management   │
└────────────────┬────────────────────────┘
                 │
┌────────────────▼────────────────────────┐
│       LangGraph Orchestrator            │
│  Router → Chat / RAG / Web / Image      │
└──┬──────────┬──────────┬────────────┬───┘
   │          │          │            │
┌──▼──┐  ┌───▼───┐  ┌───▼───┐  ┌────▼────┐
│Chat │  │  RAG  │  │  Web  │  │  Image  │
│Agent│  │ Agent │  │ Agent │  │  Agent  │
└──┬──┘  └───┬───┘  └───┬───┘  └────┬────┘
   │          │          │            │
   └──────────┴──────────┴────────────┘
                 │
┌────────────────▼────────────────────────┐
│          Groq API (LLM)                 │
│   llama-3.3-70b-versatile · llava       │
└──┬──────────────┬───────────────────────┘
   │              │
┌──▼──────┐  ┌────▼──────┐
│ FAISS   │  │  Tavily   │
│VectorDB │  │  Search   │
└─────────┘  └───────────┘
```

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/ai-assistant.git
cd ai-assistant
```

### 2. Create and activate virtual environment

```bash
# Create
python -m venv venv

# Activate (Windows)
venv\Scripts\activate

# Activate (Mac/Linux)
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Set up environment variables

```bash
# Copy the example file
cp .env.example .env
```

Now open `.env` and fill in your API keys:

| Key | Where to get it |
|---|---|
| `GROQ_API_KEY` | https://console.groq.com → API Keys |
| `TAVILY_API_KEY` | https://app.tavily.com → free tier |
| `GOOGLE_CLIENT_ID` | https://console.cloud.google.com → OAuth 2.0 |
| `GOOGLE_CLIENT_SECRET` | Same as above |
| `SECRET_KEY` | Any long random string |

### 5. Set up Google OAuth

1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create a new project
3. Go to **APIs & Services → Credentials**
4. Click **Create Credentials → OAuth 2.0 Client ID**
5. Set **Authorized redirect URIs** to `http://localhost:8501`
6. Copy the **Client ID** and **Client Secret** into your `.env`

### 6. Run the app

```bash
streamlit run main.py
```

Open your browser at `http://localhost:8501`

---

## 📁 Project Structure

```
ai-assistant/
├── app/
│   ├── agents/
│   │   ├── graph.py          # LangGraph orchestrator + streaming
│   │   ├── rag_agent.py      # PDF processing + FAISS vector search
│   │   └── web_agent.py      # Tavily web search
│   ├── pages/
│   │   ├── chat.py           # Main chat UI with streaming
│   │   └── sidebar.py        # Sidebar with history + delete
│   └── utils/
│       ├── auth.py           # Google OAuth flow
│       ├── config.py         # Environment variable loader
│       └── database.py       # SQLite chat history
├── data/                     # Auto-created (gitignored)
│   ├── chat_history.db
│   └── vectorstore/
├── .env                      # Your API keys (gitignored)
├── .env.example              # Safe template to share
├── .gitignore
├── main.py                   # App entry point
├── requirements.txt
└── README.md
```

---

## 🧠 How the AI Router Works

Every message goes through LangGraph's smart router:

```
User message
     │
     ▼
 Has image? ──── YES ──→ Image Agent (llava vision)
     │
     NO
     │
 Mode = RAG? ─── YES ──→ RAG Agent (answer from PDF)
     │
     NO
     │
 Needs web? ──── YES ──→ Web Agent (Tavily search)
     │
     NO
     │
     ▼
 Chat Agent (plain conversation)
```

---

## 🔑 API Keys — Quick Links

- **Groq** (free, fast LLM): https://console.groq.com
- **Tavily** (web search): https://app.tavily.com
- **Google OAuth**: https://console.cloud.google.com

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Streamlit |
| Orchestration | LangGraph |
| LLM | Groq (llama-3.3-70b-versatile) |
| Vision | Groq (llava-v1.5-7b) |
| Web Search | Tavily |
| Embeddings | HuggingFace sentence-transformers |
| Vector Store | FAISS |
| Database | SQLite |
| Auth | Google OAuth 2.0 |

---

## 📸 Screenshots

> Add your screenshots here after running the app.
> `![Chat UI](screenshots/chat.png)`

---

## 🤝 Contributing

Pull requests are welcome! For major changes, open an issue first.

---

## 📄 License

MIT License — free to use and modify.

---

## ⭐ If this helped you, give it a star on GitHub!
