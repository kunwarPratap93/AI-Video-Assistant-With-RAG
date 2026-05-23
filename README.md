# 🎬 AI Video Assistant

An intelligent AI-powered meeting and video analysis tool that transcribes, summarizes, and extracts actionable insights from video content and meetings using OpenAI Whisper, LangChain, and Mistral AI.

## ✨ Features

- **🎥 Multi-Source Input**: Support for YouTube URLs and local video/audio files
- **🗣️ Speech-to-Text**: Local Whisper transcription with support for Hindi-English (Hinglish) via Sarvam API
- **📝 Smart Summarization**: AI-powered meeting summaries in bullet points
- **✅ Action Item Extraction**: Automatically identifies tasks, owners, and deadlines
- **🔑 Key Decision Extraction**: Captures critical decisions made during meetings
- **❓ Question Identification**: Finds unresolved questions and follow-ups
- **💬 Interactive RAG Chat**: Ask questions about the meeting content using Retrieval-Augmented Generation
- **🎨 Beautiful UI**: Modern Streamlit interface with custom styling
- **📊 Vector Store**: Local ChromaDB for semantic search over transcripts

## 🏗️ Project Structure

```
AI-Video-Assistant/
├── app.py                          # Streamlit web UI application
├── main.py                         # CLI entry point & pipeline orchestrator
├── test.py                         # Testing & experimentation script
├── Requirements.txt                # Python dependencies
├── .env                            # API keys (not included in repo)
│
├── core/                           # Core AI processing modules
│   ├── transcriber.py             # Speech-to-text transcription (Whisper/Sarvam)
│   ├── summarizer.py              # Meeting summarization using LLM
│   ├── extractor.py               # Extract action items, decisions, questions
│   ├── rag_engine.py              # RAG pipeline for semantic Q&A
│   └── vector_store.py            # ChromaDB vector store management
│
└── utils/                          # Utility functions
    └── audio_processor.py         # Audio/video download, conversion, chunking
```

## 📋 File Descriptions

### Root Files

| File | Purpose |
|------|---------|
| **app.py** | Streamlit web interface with beautiful dark theme. Allows uploading videos/audios or pasting YouTube URLs for interactive analysis. |
| **main.py** | CLI pipeline orchestrator. Processes video → extracts transcript → generates insights → enables chat interface. |
| **test.py** | Quick testing script. Demonstrates full pipeline usage with a YouTube video example. |

### Core Modules (`core/`)

| Module | Function |
|--------|----------|
| **transcriber.py** | Converts audio to text using Whisper (local) or Sarvam API (Hindi). Handles audio chunking for API limits. |
| **summarizer.py** | Uses LangChain + Mistral to generate meeting summaries. Implements hierarchical summarization for long transcripts. |
| **extractor.py** | Extracts structured insights: action items (with owner/deadline), key decisions, and open questions. |
| **rag_engine.py** | Builds RAG chain for semantic search. Embeds transcript chunks and enables natural language Q&A. |
| **vector_store.py** | Manages ChromaDB local vector database. Handles document storage, retrieval, and persistence. |

### Utils (`utils/`)

| Module | Function |
|--------|----------|
| **audio_processor.py** | Downloads audio from YouTube, converts formats to WAV, chunks audio into manageable segments (10-min default). |

## 🚀 Quick Start

### Prerequisites

- Python 3.10+
- FFmpeg (for audio processing)
- API Keys:
  - [Mistral API Key](https://console.mistral.ai) (for LLM)
  - [Sarvam API Key](https://console.sarvam.ai) (optional, for Hindi support)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-repo/AI-Video-Assistant.git
   cd AI-Video-Assistant
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r Requirements.txt
   ```

4. **Set up environment variables**
   Create a `.env` file in the root directory:
   ```env
   MISTRAL_API_KEY=your_mistral_api_key_here
   SARVAM_API_KEY=your_sarvam_api_key_here (optional)
   WHISPER_MODEL=small  # Options: tiny, small, base, medium, large
   SARVAM_STT_MODEL=saaras:v2.5
   ```

### Usage

#### CLI Mode
```bash
python main.py
# Follow prompts to enter YouTube URL or file path
# Select language (english/hinglish)
# Interact with the chat interface
```

#### Web UI Mode
```bash
streamlit run app.py
# Opens interactive interface in browser
```

#### Testing
```bash
python test.py
# Runs a quick demo with a sample YouTube video
```

## 🔧 Configuration

### Whisper Model Sizes
| Model | Size | Speed | Accuracy |
|-------|------|-------|----------|
| tiny | ~39M | ⚡⚡⚡ | ⭐⭐ |
| small | ~140M | ⚡⚡ | ⭐⭐⭐ |
| base | ~140M | ⚡ | ⭐⭐⭐ |
| medium | ~769M | 🐢 | ⭐⭐⭐⭐ |
| large | ~2.9B | 🐢🐢 | ⭐⭐⭐⭐⭐ |

**Note**: First run downloads the model (~140MB for 'small'). Subsequent runs use cached model.

### Vector Store
- **Location**: `./vector_db/` (local ChromaDB)
- **Embedding Model**: `all-MiniLM-L6-v2` (38M parameters, fast & efficient)
- **Chunk Size**: 500 characters with 50-char overlap
- **Retriever K**: Returns 4 most relevant chunks per query

## 📊 Pipeline Architecture

```
INPUT (YouTube URL or File)
    ↓
[Audio Processor] → Download/Convert → Chunk Audio
    ↓
[Transcriber] → Whisper/Sarvam → Raw Transcript
    ↓
[Summarizer] → Hierarchical Summarization → Summary + Title
    ↓
[Extractor] → Extract Action Items → Key Decisions → Questions
    ↓
[Vector Store] → Embed Transcript → Store in ChromaDB
    ↓
[RAG Engine] → Build Retrieval Chain
    ↓
OUTPUT (JSON with all insights + RAG chain for Q&A)
```

## 🔐 API Keys Required

### Mistral AI (Required)
1. Go to [Mistral Console](https://console.mistral.ai)
2. Create an account or login
3. Navigate to API Keys section
4. Generate a new API key
5. Add to `.env` as `MISTRAL_API_KEY`

### Sarvam AI (Optional - for Hindi/Hinglish)
1. Go to [Sarvam Console](https://console.sarvam.ai)
2. Create an account
3. Generate API key
4. Add to `.env` as `SARVAM_API_KEY`

## 📦 Key Dependencies

| Package | Purpose |
|---------|---------|
| **yt-dlp** | YouTube audio download |
| **openai-whisper** | Local speech-to-text |
| **langchain** | LLM orchestration & RAG |
| **mistralai** | LLM inference |
| **chromadb** | Vector database |
| **sentence-transformers** | Embeddings generation |
| **streamlit** | Web UI framework |
| **pydub** | Audio manipulation |

## 💡 Usage Examples

### Example 1: Analyze a YouTube Video
```python
from main import run_pipeline

result = run_pipeline(
    source="https://www.youtube.com/watch?v=...",
    language="english"
)

print(result['title'])
print(result['summary'])
print(result['action_items'])
```

### Example 2: Ask Questions About a Meeting
```python
rag_chain = result['rag_chain']
answer = ask_question(rag_chain, "What were the main action items?")
```

### Example 3: Local File Processing
```python
from main import run_pipeline

result = run_pipeline(
    source="/path/to/meeting.mp4",
    language="english"
)
```

## 🎯 Supported Input Formats

- **Video**: MP4, MKV, AVI, MOV, WebM
- **Audio**: MP3, WAV, M4A, FLAC, OGG
- **URLs**: YouTube links

## 📈 Output Format

The pipeline returns a dictionary with:

```python
{
    "title": "Meeting Title Generated by AI",
    "transcript": "Full transcribed text...",
    "summary": "Bullet-point summary...",
    "action_items": "Numbered list with owners & deadlines...",
    "key_decisions": "Numbered list of decisions...",
    "open_questions": "Numbered list of unresolved questions...",
    "rag_chain": <RAG retrieval chain for Q&A>
}
```

## 🐛 Troubleshooting

### Issue: "FFmpeg not found"
**Solution**: Install FFmpeg
- **Ubuntu/Debian**: `sudo apt-get install ffmpeg`
- **macOS**: `brew install ffmpeg`
- **Windows**: Download from [ffmpeg.org](https://ffmpeg.org/download.html)

### Issue: "CUDA out of memory" with Whisper
**Solution**: Use smaller model
```env
WHISPER_MODEL=small  # or 'tiny'
```

### Issue: "API Key invalid"
**Solution**: Verify keys in `.env` file are correct and have proper format

### Issue: Transcription is slow
**Solution**: Check internet connection, reduce audio length, or use smaller Whisper model

## 🤝 Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see LICENSE file for details.

## 🙏 Acknowledgments

- **OpenAI** - Whisper speech-to-text model
- **Mistral AI** - LLM provider
- **LangChain** - LLM orchestration framework
- **ChromaDB** - Vector database
- **HuggingFace** - Embedding models
- **Streamlit** - Web UI framework

## 📞 Support

For issues, questions, or feature requests, please:
- Open an issue on GitHub
- Check existing documentation
- Review error messages carefully

---

**Last Updated**: May 2026  
**Status**: Active Development  
**Python Version**: 3.10+
