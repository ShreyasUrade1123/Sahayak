# AI Sahayak Voice - Intelligent Voice Generation Platform

[![Next.js](https://img.shields.io/badge/Next.js-15.1-black?logo=next.js&logoColor=white)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=black)](https://reactjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Python](https://img.shields.io/badge/Python-3.8+-3776AB?logo=python&logoColor=white)](https://python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-Latest-009688?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)

> A production-ready AI voice generation platform combining text-to-speech, multilingual translation, and voice cloning. Built with Next.js 15, React 19, and powered by Kokoro TTS and YourTTS models.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Screenshots](#screenshots)
- [System Architecture](#system-architecture)
- [Tech Stack](#tech-stack)
- [Quick Start](#quick-start)
- [Installation](#installation)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Configuration](#configuration)
- [Deployment](#deployment)
- [Troubleshooting](#troubleshooting)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

**AI Sahayak Voice** is a full-stack AI-powered voice generation platform that enables:

- **Text-to-Speech**: Convert text to natural speech in 9 languages with automatic translation
- **Voice Cloning**: Clone any voice from audio samples with multilingual support
- **Real-time Processing**: Browser-based recording and instant audio generation

Built for developers, content creators, and businesses needing advanced voice AI capabilities.

---

## Features

### Text-to-Speech
- Automatic language detection and translation (9 languages)
- Male/female voice selection per language
- High-quality 16kHz WAV output with Kokoro TTS
- Translation preview before generation

### Voice Cloning
- Browser-based audio recording or file upload
- Clone voices in English, Hindi, French, Spanish
- YourTTS model for high-quality cloning
- 5-15 second processing time

### User Interface
- Modern dark theme with responsive design
- Radix UI components with Tailwind CSS
- Framer Motion animations
- Real-time audio preview and download

**Supported Languages**: English, Spanish, French, Hindi, Italian, Portuguese, Japanese, Chinese (Simplified/Traditional)

---

## Screenshots

### Landing Page
![Landing Page](./Screenshot%202025-11-02%20075029.png)
*Modern hero section with feature highlights and call-to-action*

### Text-to-Speech Interface
![Text-to-Speech](./Screenshot%202025-11-02%20075046.png)
*Multilingual text-to-speech with automatic translation and voice selection*

### Voice Cloning Interface
![Voice Cloning](./Screenshot%202025-11-02%20075108.png)
*Real-time audio recording and voice cloning with 4-language support*

---

## System Architecture

AI Sahayak Voice follows a distributed microservices architecture with clear separation between frontend, tunneling, and AI processing layers.

```
┌─────────────────────────────────────────────────────────────────────────┐
│                          PRESENTATION LAYER                              │
│                     (Next.js 15 + React 19 + TypeScript)                │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  ┌───────────────────────────┐    ┌─────────────────────────────────┐  │
│  │   Text-to-Speech UI       │    │   Voice Cloning UI              │  │
│  │                           │    │                                 │  │
│  │  • Language Selector      │    │  • Audio Recorder/Uploader      │  │
│  │  • Voice Gender Selector  │    │  • Language Selector (4 langs)  │  │
│  │  • Translation Preview    │    │  • Real-time Recording          │  │
│  │  • Audio Player           │    │  • Audio Player & Download      │  │
│  └───────────────────────────┘    └─────────────────────────────────┘  │
│                                                                          │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │               Shared Components Layer                           │   │
│  │  • Radix UI (50+ components)  • React Hook Form  • Zod         │   │
│  │  • Audio Utilities            • Theme Provider   • Toasts       │   │
│  └─────────────────────────────────────────────────────────────────┘   │
└────────────────────────────┬────────────────────────────────────────────┘
                             │
                             │ HTTPS Requests
                             │
┌────────────────────────────▼────────────────────────────────────────────┐
│                        TUNNELING LAYER                                   │
│                          (ngrok Free Tier)                               │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  ┌────────────────────────────────┐    ┌──────────────────────────────┐│
│  │  TTS ngrok Tunnel              │    │  Voice Clone ngrok Tunnel    ││
│  │  https://*.ngrok-free.app      │    │  https://*.ngrok-free.app    ││
│  │  • HTTPS Termination           │    │  • HTTPS Termination         ││
│  │  • Dynamic URL Management      │    │  • Dynamic URL Management    ││
│  └────────────────────────────────┘    └──────────────────────────────┘│
└────────────────────────────┬───────────────────┬────────────────────────┘
                             │                   │
┌────────────────────────────▼───────────────────▼────────────────────────┐
│                        BACKEND PROCESSING LAYER                          │
│                     (FastAPI + Python on Google Colab)                  │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  ┌──────────────────────────────────┐  ┌───────────────────────────────┐│
│  │     TTS Backend Service          │  │  Voice Cloning Backend        ││
│  │     (Port: 8000)                 │  │  (Port: 8001)                 ││
│  │                                  │  │                               ││
│  │  1. Language Detection           │  │  1. Audio File Upload         ││
│  │     └─ langdetect library        │  │     └─ multipart/form-data    ││
│  │                                  │  │                               ││
│  │  2. Translation Engine           │  │  2. Audio Processing          ││
│  │     └─ deep-translator (9 langs) │  │     └─ WAV format validation  ││
│  │     └─ LibreTranslate API        │  │     └─ Sample rate check      ││
│  │                                  │  │                               ││
│  │  3. Voice Mapping                │  │  3. Voice Cloning Model       ││
│  │     ├─ Male: af_sky              │  │     └─ Coqui TTS (YourTTS)    ││
│  │     └─ Female: af_bella          │  │     └─ Multi-speaker support  ││
│  │                                  │  │     └─ 4 language models      ││
│  │  4. TTS Generation               │  │                               ││
│  │     └─ Kokoro-82M Model          │  │  4. Audio Generation          ││
│  │     └─ 16kHz WAV output          │  │     └─ Text + Voice synthesis ││
│  │                                  │  │     └─ 16kHz WAV output       ││
│  └──────────────────────────────────┘  └───────────────────────────────┘│
│                                                                          │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                    Infrastructure Services                      │   │
│  │  • uvicorn (ASGI server)    • nest-asyncio (async support)     │   │
│  │  • numpy (audio processing)  • soundfile (WAV handling)         │   │
│  │  • scipy (signal processing) • pandas (data handling)           │   │
│  └─────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────┘
```

### Component Overview

**Frontend Layer**:
- **Next.js App Router**: Server-side rendering with React 19 Server Components
- **TypeScript 5**: Type-safe API calls and component props
- **Radix UI + Tailwind**: Accessible, responsive component library
- **Audio APIs**: MediaRecorder for recording, Web Audio API for playback

**Tunneling Layer**:
- **ngrok**: Exposes local Colab backends to public HTTPS endpoints
- **Dynamic URLs**: Regenerated on each Colab session restart
- **CORS Handling**: Configured for cross-origin requests

**Backend Services**:
- **FastAPI**: Async request handling with automatic API docs
- **Kokoro TTS (82M parameters)**: Lightweight, fast text-to-speech model
- **YourTTS**: Multi-speaker voice cloning with speaker embedding
- **deep-translator**: LibreTranslate integration for 9 languages

### Data Flow Examples

**Text-to-Speech Flow**:
```
1. User enters "Hello" in English
2. Frontend detects language using langdetect
3. User selects target: French (fr_FR)
4. Translation service converts: "Hello" → "Bonjour"
5. Voice mapper selects: af_sky (male voice)
6. Kokoro TTS generates audio from "Bonjour"
7. Backend returns WAV blob (16kHz, mono)
8. Frontend plays audio via Web Audio API
9. User downloads as bonjour_speech.wav
```

**Voice Cloning Flow**:
```
1. User records 8-second audio sample in browser
2. MediaRecorder captures as WebM blob
3. Frontend converts WebM → WAV (client-side)
4. User enters text: "This is my voice"
5. Uploads WAV + text + language (en) to backend
6. YourTTS extracts speaker embeddings from audio
7. Model synthesizes text in user's voice characteristics
8. Backend returns cloned audio as WAV blob
9. Frontend plays and offers download
```

---

## Tech Stack

| Category | Technologies |
|----------|-------------|
| **Frontend** | Next.js 15.1, React 19, TypeScript 5 |
| **Styling** | Tailwind CSS 3.4, Radix UI, Framer Motion |
| **Backend** | Python, FastAPI, Uvicorn |
| **AI Models** | Kokoro TTS, Coqui TTS (YourTTS), deep-translator |
| **Infrastructure** | ngrok, Google Colab, Vercel |
| **Form/State** | React Hook Form, Zod, Next-themes |

---

## Quick Start

Get the platform running in 4 steps:

### 1. Start Backend Services

**Google Colab (Recommended):**
1. Open [TTS_NGROK_BACKEND.ipynb](./TTS_NGROK_BACKEND.ipynb) in Colab
2. Update ngrok token and run all cells
3. Open [Voice_Cloning (1).ipynb](./Voice_Cloning%20(1).ipynb) in Colab
4. Update ngrok token and run all cells
5. Copy both ngrok URLs

### 2. Configure Frontend

Create `.env.local`:
```env
NEXT_PUBLIC_TTS_API_URL=https://your-tts-backend.ngrok-free.app
NEXT_PUBLIC_VOICE_CLONE_API_URL=https://your-voice-clone-backend.ngrok-free.app
```

### 3. Install & Run

```bash
npm install
npm run dev
```

### 4. Test

Navigate to `http://localhost:3000` and try:
- Text-to-Speech: Enter text → Select language → Generate
- Voice Cloning: Record/upload audio → Enter text → Clone

---

## Installation

### Prerequisites

- Node.js 22+ ([Download](https://nodejs.org/))
- Python 3.8+ (for local backend)
- ngrok account ([Sign up](https://ngrok.com/))

### Frontend Setup

```bash
# Clone repository
git clone https://github.com/yourusername/AI-Sahayak-Voice.git
cd AI-Sahayak-Voice-main

# Install dependencies
npm install

# Create environment file
cp .env.example .env.local
# Edit .env.local with your backend URLs
```

### Backend Setup

**Option A: Google Colab (Easiest)**

1. Open both Jupyter notebooks in Colab
2. Update ngrok auth tokens in cells
3. Run all cells in each notebook
4. Copy generated ngrok URLs

**Option B: Local Setup**

```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Install TTS dependencies
pip install fastapi uvicorn pyngrok deep-translator langdetect soundfile kokoro numpy nest-asyncio

# Install Voice Cloning dependencies
pip install TTS python-multipart pandas==2.2.2 scipy

# Run backends (extract from notebooks)
python tts_server.py
python voice_clone_server.py
```

### Update Frontend Configuration

Update API URLs in:
- `.env.local` (recommended), OR
- [components/audio/text-to-speech-interface.tsx](./components/audio/text-to-speech-interface.tsx) (line 10)
- [components/audio/voice-cloning-interface.tsx](./components/audio/voice-cloning-interface.tsx) (line 80)

---

## Usage

### Text-to-Speech

```typescript
// 1. Navigate to /text-to-speech
// 2. Enter text: "Hello, how are you?"
// 3. Select target language (e.g., French)
// 4. Choose voice gender (Male/Female)
// 5. Click "Generate Audio"
// 6. Play or download the result
```

**Example Translation Flow:**
- Input: "Hello" (English detected)
- Target: French (fr)
- Output: "Bonjour" (spoken in French)

### Voice Cloning

```typescript
// 1. Navigate to /voice-cloning
// 2. Record audio (5-10 seconds) or upload .wav file
// 3. Enter text to speak: "This is my cloned voice"
// 4. Select language (en/hi/fr/es)
// 5. Click "Clone Audio"
// 6. Listen to cloned voice result
```

**Tips for Best Results:**
- Use clear, noise-free audio samples
- 5-10 seconds of audio is optimal
- Speak naturally with consistent volume

---

## Project Structure

```
AI-Sahayak-Voice-main/
├── app/                              # Next.js App Router
│   ├── globals.css                   # Global styles
│   ├── layout.tsx                    # Root layout
│   ├── page.tsx                      # Home page
│   ├── text-to-speech/
│   │   └── page.tsx                  # TTS page
│   └── voice-cloning/
│       └── page.tsx                  # Voice cloning page
│
├── components/
│   ├── audio/                        # Audio features
│   │   ├── audio-sidebar.tsx
│   │   ├── text-to-speech-interface.tsx  # TTS UI (API: line 10)
│   │   └── voice-cloning-interface.tsx   # Cloning UI (API: line 80)
│   ├── layouts/
│   │   └── main-layout.tsx
│   ├── navigation/
│   │   ├── navbar.tsx
│   │   └── footer.tsx
│   ├── sections/                     # Landing page
│   │   ├── hero-section.tsx
│   │   ├── features-section.tsx
│   │   ├── clients-section.tsx
│   │   ├── email-signup-section.tsx
│   │   └── audio-generation-showcase.tsx
│   ├── ui/                           # Radix UI components (50+)
│   │   ├── button.tsx
│   │   ├── card.tsx
│   │   ├── dialog.tsx
│   │   ├── input.tsx
│   │   ├── select.tsx
│   │   └── ...
│   └── theme-provider.tsx
│
├── hooks/                            # Custom hooks
│   ├── use-mobile.tsx
│   └── use-toast.ts
│
├── lib/
│   └── utils.ts                      # Utilities
│
├── public/                           # Static assets
│
├── Backend Notebooks/
│   ├── TTS_NGROK_BACKEND.ipynb       # TTS backend
│   │   # - Kokoro TTS model
│   │   # - Translation integration
│   │   # - ngrok tunnel
│   └── Voice_Cloning (1).ipynb       # Cloning backend
│       # - YourTTS model
│       # - Audio file handling
│       # - ngrok tunnel
│
├── Configuration Files/
│   ├── package.json                  # Dependencies
│   ├── tsconfig.json                 # TypeScript config
│   ├── next.config.mjs               # Next.js config
│   ├── tailwind.config.ts            # Tailwind config
│   ├── components.json               # UI components config
│   └── .gitignore
│
├── .env.local                        # Environment variables (create this)
├── .env.example                      # Environment template
└── README.md                         # This file
```

**Key Files:**
- **components/audio/text-to-speech-interface.tsx** - TTS UI logic (update API URL at line 10)
- **components/audio/voice-cloning-interface.tsx** - Cloning UI logic (update API URL at line 80)
- **TTS_NGROK_BACKEND.ipynb** - TTS FastAPI backend (contains ngrok token)
- **Voice_Cloning (1).ipynb** - Voice cloning FastAPI backend (contains ngrok token)

---

## Configuration

### Environment Variables

Create `.env.local` in project root:

```env
# Backend API URLs
NEXT_PUBLIC_TTS_API_URL=https://your-tts-backend.ngrok-free.app
NEXT_PUBLIC_VOICE_CLONE_API_URL=https://your-voice-clone-backend.ngrok-free.app

# Optional
NEXT_PUBLIC_MAX_TEXT_LENGTH=5000
NEXT_PUBLIC_MAX_AUDIO_DURATION=60
```

### Supported Audio Formats

**Input (Voice Cloning):**
- WAV (recommended): Mono, 16kHz
- Browser Recording: WebM (auto-converted to WAV)

**Output:**
- WAV: Mono, 16kHz, 16-bit PCM

### API Endpoints

**TTS Backend:**
```bash
POST /tts
Content-Type: application/json
{
  "text": "Hello world",
  "voice": "af_sky",  # or "af_bella" for female
  "speed": 1.0
}
```

**Voice Cloning Backend:**
```bash
POST /clone
Content-Type: multipart/form-data
{
  "file": <audio_file.wav>,
  "text": "Text to speak",
  "language": "en"  # en, hi, fr, es
}
```

---

## Deployment

### Frontend (Vercel)

```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel

# Add environment variables in Vercel dashboard:
# NEXT_PUBLIC_TTS_API_URL
# NEXT_PUBLIC_VOICE_CLONE_API_URL
```

### Backend

**Production Options:**
- **Railway**: Deploy Python apps with persistent URLs
- **Render**: Free tier with automatic deployments
- **Google Cloud Run**: Serverless containers
- **AWS EC2**: Full control for production

**Note**: Google Colab is for development only (sessions timeout after 12 hours).

---

## Troubleshooting

### "Failed to fetch" Error

**Cause**: Backend not accessible or ngrok URL changed

**Solution**:
1. Check both Colab notebooks are running
2. Verify ngrok URLs haven't changed (they reset on notebook restart)
3. Update frontend `.env.local` with new URLs
4. Restart frontend: `npm run dev`

### "Audio Not Playing"

**Cause**: Browser compatibility or audio format issue

**Solution**:
1. Test in Chrome/Edge (best support)
2. Check browser console for errors
3. Verify audio blob size: `console.log(audioBlob.size)`
4. Ensure backend returns WAV format

### "Model Loading Failed"

**Cause**: First request takes time to load models

**Solution**:
1. Wait 30-60 seconds for first request
2. In Colab: Enable GPU (Runtime > Change runtime type)
3. If OOM error: Restart runtime

### "ngrok Tunnel Not Working"

**Cause**: Invalid token or tunnel limit reached

**Solution**:
1. Verify ngrok auth token is correct
2. Free ngrok allows 1 tunnel - close other tunnels
3. Re-run ngrok setup cell in Colab
4. Check firewall settings

**More Issues?** [Open an issue](https://github.com/yourusername/AI-Sahayak-Voice/issues)

---

## Roadmap

### Phase 1: Core Features ✅ (Completed)
- [x] Text-to-Speech with Kokoro TTS integration
- [x] 9-language translation support (English, Spanish, French, Hindi, Italian, Portuguese, Japanese, Chinese)
- [x] Voice cloning with YourTTS model
- [x] Real-time audio recording in browser
- [x] Modern UI with Radix UI and Tailwind CSS
- [x] Google Colab backend deployment with ngrok
- [x] Vercel frontend deployment

### Phase 2: Enhancements 🚧 (In Progress)
- [ ] Batch audio generation (multiple texts at once)
- [ ] Audio editing tools (trim, normalize, effects)
- [ ] Voice gallery (pre-recorded voice samples)
- [ ] Export formats: MP3, OGG, FLAC
- [ ] User accounts and audio history
- [ ] API rate limiting and usage analytics

### Phase 3: Advanced Features 🔮 (Planned)
- [ ] Real-time voice conversion (live microphone input)
- [ ] Custom voice training (user-uploaded datasets)
- [ ] Speech-to-Text transcription
- [ ] Multi-speaker conversations (different voices)
- [ ] SSML support for advanced speech control
- [ ] Emotion/tone selection (happy, sad, excited)
- [ ] Background music integration
- [ ] Podcast generation tools

### Phase 4: AI Enhancements 🤖 (Future)
- [ ] GPT integration for script generation
- [ ] Automatic voice matching (find similar voices)
- [ ] Pronunciation correction AI
- [ ] Accent transfer (speak English with French accent)
- [ ] Voice aging (make voice sound younger/older)
- [ ] Lip-sync video generation
- [ ] Voice beautification filters
- [ ] Real-time translation + voice cloning

**Want to contribute?** Check the [Contributing](#contributing) section or pick an item from the roadmap!

---

## Contributing

Contributions are welcome! Here's how:

1. **Fork** the repository
2. **Create** a feature branch: `git checkout -b feature/amazing-feature`
3. **Commit** your changes: `git commit -m "Add amazing feature"`
4. **Push** to branch: `git push origin feature/amazing-feature`
5. **Open** a Pull Request

### Development Guidelines

- Follow existing TypeScript/React patterns
- Test on Chrome, Firefox, Safari
- Update README for new features
- Keep code clean and documented

---

## License

This project is licensed under the **MIT License**.

```
MIT License

Copyright (c) 2025 AI Sahayak Voice

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## Acknowledgments

Built with:
- **AI Models**: [Kokoro TTS](https://github.com/hexgrad/Kokoro-82M), [Coqui TTS](https://github.com/coqui-ai/TTS)
- **Frameworks**: Next.js, React, FastAPI
- **UI Libraries**: Radix UI, Tailwind CSS, Framer Motion
- **Infrastructure**: ngrok, Google Colab, Vercel

---

## Author

**Shreyas Urade**

- GitHub: [@shreyasurade](https://github.com/ShreyasUrade1123)
- LinkedIn: [Shreyas Urade](https://www.linkedin.com/in/shreyas-urade-3ba072258/)
- Email: shreyasurade4940@gmail.com

---

## Support

- **Documentation**: This README
- **Issues**: [GitHub Issues](https://github.com/yourusername/AI-Sahayak-Voice/issues)
- **Discussions**: [GitHub Discussions](https://github.com/yourusername/AI-Sahayak-Voice/discussions)

---

**Built with ❤️ using Next.js 15, React 19**

For questions or collaboration: [shreyas.urade@example.com](mailto:shreyasurade4940@gmail.com)
