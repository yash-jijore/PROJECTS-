# 🎬 VidsnapAI

> Transform your images into stunning AI-narrated short reels — automatically.

VidsnapAI is a web application that lets users upload a set of images along with a description, and automatically generates a short vertical reel (1080×1920) with an AI-generated voiceover using ElevenLabs TTS — ready to share on Instagram, YouTube Shorts, or TikTok.

---

## ✨ Features

- 📸 **Upload multiple images** to build your reel slideshow
- 🗣️ **AI Voiceover** — automatically converts your description to speech using ElevenLabs
- 🎞️ **Auto Reel Generation** — stitches images + audio into a 1080×1920 MP4 using FFmpeg
- 🖼️ **Gallery View** — browse all generated reels in one place
- ⚙️ **Background Processing** — a queue-based worker handles generation without blocking the UI

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Backend | Python, Flask |
| TTS (Voice) | ElevenLabs API |
| Video Processing | FFmpeg |
| Frontend | HTML/Jinja2 Templates |
| File Handling | Werkzeug |

---

## 📁 Project Structure

```
VidsnapAI/
│
├── main.py                  # Flask web server — routes for home, create, gallery
├── generate_process.py      # Background worker — monitors uploads & generates reels
├── text_to_audio.py         # ElevenLabs TTS integration
├── config.py                # API key configuration
│
├── user_uploads/            # Uploaded images + generated audio per session (UUID folders)
│   └── <uuid>/
│       ├── image1.jpg
│       ├── desc.txt         # User's description (used for TTS)
│       ├── input.txt        # FFmpeg concat file list
│       └── audio.mp3        # Generated voiceover
│
├── static/
│   └── reels/               # Final generated MP4 reels
│
├── templates/
│   ├── index.html
│   ├── create.html
│   └── gallery.html
│
└── done.txt                 # Tracks already-processed session UUIDs
```

---

## ⚙️ Setup & Installation

### Prerequisites

- Python 3.8+
- FFmpeg installed and available in your system PATH
- An [ElevenLabs](https://elevenlabs.io) API key

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/VidsnapAI.git
cd VidsnapAI
```

### 2. Install Python Dependencies

```bash
pip install flask elevenlabs werkzeug
```

### 3. Configure Your API Key

Open `config.py` and replace the placeholder with your ElevenLabs API key:

```python
ELEVENLABS_API_KEY = "your_elevenlabs_api_key_here"
```

> ⚠️ **Never commit your API key to GitHub.** Use environment variables or a `.env` file in production.

### 4. Create Required Directories

```bash
mkdir -p user_uploads static/reels
```

### 5. Run the Flask App

```bash
python main.py
```

### 6. Run the Background Reel Generator

Open a **second terminal** and run:

```bash
python generate_process.py
```

This worker continuously monitors `user_uploads/` for new sessions and processes them into reels.

---

## 🚀 How to Use

1. Open your browser and go to `http://localhost:5000`
2. Click **Create** to start a new reel
3. Upload your images and write a description for the voiceover
4. Submit — the background worker will:
   - Convert your description to speech (ElevenLabs)
   - Stitch images + audio into a vertical MP4 (FFmpeg)
5. Visit the **Gallery** page to view and download your generated reel

---

## 🎬 How Reel Generation Works

```
User uploads images + description
        ↓
Flask saves files to user_uploads/<uuid>/
        ↓
generate_process.py detects new folder
        ↓
text_to_audio.py → ElevenLabs API → audio.mp3
        ↓
FFmpeg stitches images + audio → 1080x1920 MP4
        ↓
Reel saved to static/reels/<uuid>.mp4
        ↓
Available in Gallery 🎉
```

---

## 📌 Important Notes

- Each user session is assigned a unique UUID to keep uploads isolated
- The `done.txt` file tracks processed sessions to avoid re-processing
- FFmpeg output is always scaled and padded to **1080×1920** (portrait/vertical format)
- The ElevenLabs voice used is **Adam** (`pNInz6obpgDQGcFmaJgB`) with the `eleven_turbo_v2_5` model for low latency

---

## 🔒 Security Recommendations

- Move `ELEVENLABS_API_KEY` to an environment variable:
  ```python
  import os
  ELEVENLABS_API_KEY = os.environ.get("ELEVENLABS_API_KEY")
  ```
- Add `.env` and `user_uploads/` to your `.gitignore`
- Disable Flask debug mode (`debug=False`) before deploying to production

---

## 📄 .gitignore (Recommended)

```
user_uploads/
static/reels/
done.txt
config.py
__pycache__/
*.pyc
.env
```

---

## 🤝 Contributing

Pull requests are welcome! For major changes, please open an issue first to discuss what you'd like to change.

---

---

<p align="center">Made with ❤️ using Flask, ElevenLabs & FFmpeg</p>
