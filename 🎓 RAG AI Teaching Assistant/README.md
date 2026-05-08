# 🎓 RAG AI Teaching Assistant

> Ask questions about your video course — get answers with exact video titles and timestamps.

A fully local **Retrieval-Augmented Generation (RAG)** pipeline that turns any video course into an interactive AI assistant. Students can ask natural language questions and get guided to the exact video and timestamp where the topic is covered — no internet required, runs entirely on your machine.

---

## 🧠 How It Works

```
Video files (.mp4 / .mkv)
        ↓  [video_to_mp3.py]
Audio files (.mp3)
        ↓  [mp3_to_json.py]
Transcribed JSON chunks (Whisper large-v2)
        ↓  [preprocess_json.py]
Embeddings via Ollama (bge-m3) → embeddings.joblib
        ↓  [process_incoming.py]
User Query → Embed → Cosine Similarity → Top 5 Chunks
        ↓
Prompt built → LLaMA 3.2 via Ollama → Human-readable Answer 🎯
```

---

## ✨ Features

- 🎥 **Video-to-text pipeline** — automatically extracts audio and transcribes using OpenAI Whisper
- 🌐 **Multilingual support** — transcribes non-English audio (e.g., Hindi) and translates to English
- 🔢 **Timestamped chunks** — every subtitle segment is stored with start/end time and video metadata
- 🧬 **Local embeddings** — uses `bge-m3` via Ollama for high-quality semantic embeddings
- 🔍 **Semantic search** — cosine similarity finds the most relevant video segments for any question
- 🤖 **LLM-powered answers** — LLaMA 3.2 generates natural, human-friendly responses with video references
- 🔒 **Fully local & private** — no API keys, no data sent to the cloud

---

## 🛠️ Tech Stack

| Component | Technology |
|---|---|
| Transcription | OpenAI Whisper (`large-v2`) |
| Embeddings | Ollama — `bge-m3` model |
| LLM Inference | Ollama — `llama3.2` model |
| Similarity Search | scikit-learn `cosine_similarity` |
| Data Storage | pandas DataFrame + joblib |
| Audio Extraction | FFmpeg |

---

## 📁 Project Structure

```
RAG-Teaching-Assistant/
│
├── video_to_mp3.py        # Step 1 — Extract audio from video files using FFmpeg
├── mp3_to_json.py         # Step 2 — Transcribe audio to timestamped JSON using Whisper
├── preprocess_json.py     # Step 3 — Generate embeddings and save as embeddings.joblib
├── process_incoming.py    # Step 4 — Query handler: embed → search → prompt → LLM response
│
├── videos/                # Place your raw video course files here
├── audios/                # Auto-generated MP3 files
├── jsons/                 # Auto-generated Whisper transcription JSONs
│
├── embeddings.joblib      # Generated vector store (after preprocessing)
├── prompt.txt             # Last generated prompt (for debugging)
├── response.txt           # Last LLM response (for debugging)
│
└── README.md
```

---

## ⚙️ Setup & Installation

### Prerequisites

- Python 3.8+
- [FFmpeg](https://ffmpeg.org/download.html) installed and in PATH
- [Ollama](https://ollama.com) installed and running locally

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/RAG-Teaching-Assistant.git
cd RAG-Teaching-Assistant
```

### 2. Install Python Dependencies

```bash
pip install openai-whisper pandas scikit-learn numpy joblib requests
```

### 3. Pull Required Ollama Models

```bash
ollama pull bge-m3
ollama pull llama3.2
```

### 4. Create Required Directories

```bash
mkdir -p videos audios jsons
```

---

## 🚀 Usage (Step-by-Step)

### Step 1 — Add Your Videos

Place your video files inside the `videos/` folder. Files should follow this naming format:

```
<Title> #<Number> [<extra info>] ｜ <Channel>.mp4
```

Example:
```
Introduction to CSS #14 [abc123] ｜ MyChannel.mp4
```

### Step 2 — Extract Audio

```bash
python video_to_mp3.py
```

Converts each video to an MP3 file in `audios/`, named as `<number>_<title>.mp3`.

### Step 3 — Transcribe Audio to JSON

```bash
python mp3_to_json.py
```

Uses Whisper `large-v2` to transcribe (and translate if needed) each audio file into timestamped segment chunks saved in `jsons/`.

> 💡 Supports multilingual audio — set `language="hi"` for Hindi, or change to your language code.

### Step 4 — Generate Embeddings

```bash
python preprocess_json.py
```

Sends all text chunks to Ollama's `bge-m3` model to generate semantic embeddings, then saves everything as `embeddings.joblib`.

### Step 5 — Ask Questions!

```bash
python process_incoming.py
```

Enter your question when prompted. The assistant will:
1. Embed your question
2. Find the top 5 most relevant video segments
3. Build a context-aware prompt
4. Return a human-friendly answer with video titles and timestamps

---

## 💬 Example

**Query:**
```
where is html concluded in this course?
```

**Response:**
```
HTML is concluded across a couple of videos in the course!

- Video 13: "Entities, Code tag and more on HTML" — HTML is wrapped up starting around
  the 55-second mark, with a final conclusion at ~520 seconds.

- Video 14: "Introduction to CSS" — kicks off by confirming HTML is fully done,
  right at the beginning (~5 seconds in).

I'd recommend jumping to Video 13 first, then Video 14 to pick up where CSS begins!
```

---

## 🔧 Configuration

You can customize the following in the scripts:

| Setting | File | Default | Description |
|---|---|---|---|
| Whisper model size | `mp3_to_json.py` | `large-v2` | Change to `base` or `medium` for faster processing |
| Source language | `mp3_to_json.py` | `hi` (Hindi) | Set to your audio language code |
| Embedding model | `preprocess_json.py` | `bge-m3` | Any Ollama-supported embedding model |
| LLM model | `process_incoming.py` | `llama3.2` | Swap for `deepseek-r1` or any Ollama model |
| Top results returned | `process_incoming.py` | `5` | Number of chunks used as context |

---

## 📌 Notes

- Whisper `large-v2` gives the best accuracy but is slower — use `base` for quick testing
- Make sure Ollama is running (`ollama serve`) before running `preprocess_json.py` or `process_incoming.py`
- The `embeddings.joblib` file only needs to be regenerated if you add new videos
- `prompt.txt` and `response.txt` are saved after each query — useful for debugging LLM responses

---

## 🤝 Contributing

Pull requests are welcome! Ideas for improvement include adding a web UI (Flask/Streamlit), support for direct YouTube URL ingestion, or a vector database like ChromaDB for larger course libraries.

---

## 📝 License

This project is licensed under the MIT License.

---

<p align="center">Made with 🎓 using Whisper, Ollama & scikit-learn — runs 100% locally</p>
