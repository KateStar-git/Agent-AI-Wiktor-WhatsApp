# Agent Wiktor - WhatsApp AI Assistant with Voice & RAG Support

An advanced, production-ready AI Agent workflow built in **n8n**. "Agent Wiktor" acts as a smart, personal assistant integrated directly with WhatsApp. It is capable of understanding both text and voice messages, maintaining conversation context, and fetching precise answers from a vector knowledge base.

## 🚀 Features

- **Full Voice-to-Voice & Text Support**: 
  - Transcribes incoming WhatsApp voice notes using OpenAI Whisper.
  - Generates realistic voice responses using OpenAI Text-to-Speech (TTS) with `onyx` voice.
- **RAG Architecture (Retrieval-Augmented Generation)**: Uses **Supabase Vector Store** and OpenAI Embeddings to search the Adaptify AI company knowledge base before answering.
- **Smart Session Memory**: Integrated with **PostgreSQL Chat Memory** to retain context across the last 10 interactions per user phone number.
- **Robust Routing**: Advanced switching logic implemented to seamlessly branch between processing text and handling asynchronous binary audio downloads from Meta's API.
- **Data Transformation**: Custom JavaScript node handles low-level MIME-type correction (`audio/mp3` to `audio/mpeg`) ensuring 100% compatibility with WhatsApp's voice player.

## 🛠️ Tech Stack

* **Orchestration:** n8n (Advanced Workflow Automation)
* **LLM & AI Models:** OpenAI (GPT-4.1-mini / GPT-4o, Whisper, TTS)
* **Vector Database:** Supabase
* **Chat Memory:** PostgreSQL
* **Integration:** WhatsApp Business API (via Meta Cloud API)
* **Scripting:** JavaScript (Node.js context inside n8n)

## 📋 Architecture Flow

1. **Trigger:** WhatsApp Webhook receives a new message event.
2. **Router (Switch):** Separates text inputs from audio inputs.
3. **Audio Handling (If voice):** Downloads media via HTTP Request -> Transcribes via Whisper -> Passes text to Agent.
4. **Agent Processing:** LangChain Agent analyzes query -> Searches Supabase Vector Store -> Fetches chat history from Postgres -> Formulates response.
5. **Response Router (If):** Sends plain text back OR passes text to OpenAI TTS -> Fixes audio MIME-type via JS -> Sends audio message back.
