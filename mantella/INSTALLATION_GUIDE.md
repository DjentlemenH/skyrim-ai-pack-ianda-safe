# Mantella Installation Guide

## Overview
Mantella is the core AI dialogue engine for this mod pack. It provides procedural dialogue generation using local or cloud-based LLMs (Large Language Models) and text-to-speech systems.

## Prerequisites

### Required
- **SKSE64** (Skyrim Script Extender) - Latest version
- **Mod Organizer 2** or **Vortex** mod manager
- **~10GB free disk space** (for local LLM models)
- **Fuz Ro D-oh** (for subtitle/audio sync)

### Recommended
- **SSD** for Skyrim installation (improves AI response times)
- **16GB+ RAM** (for running local LLMs smoothly)
- **Stable internet connection** (if using cloud LLMs)

---

## Installation Steps

### Step 1: Install Mantella Mod Files

1. **Download Mantella** from [Nexus Mods](https://www.nexusmods.com/skyrimspecialedition/mods/98631)
2. Install via Mod Organizer 2 or Vortex:
   - In MO2: Click "Install Mod" → select downloaded archive
   - In Vortex: Drop file into Vortex mod staging area
3. **Enable Mantella.esp** in your load order
4. **Place after** any major quest/NPC mods but before AI voice packs

### Step 2: Install Mantella Software (Exe)

1. Download the Mantella executable from the Nexus files section
2. Extract to a dedicated folder (e.g., `C:\Modding\Mantella`)
3. **Do NOT place inside** your Skyrim or MO2 folders
4. Run `Mantella.exe` once to generate config files

---

## LLM Backend Setup

Choose **ONE** of the following options:

### Option A: LM Studio (Recommended for Beginners)

**Pros**: Easy GUI, good performance, automatic model management

1. Download [LM Studio](https://lmstudio.ai)
2. Install and launch LM Studio
3. In LM Studio:
   - Click "Discover" tab
   - Search for `gemma-2-4b-it` or `llama-3.2-3b-instruct`
   - Click Download (this will take 5-15 minutes)
4. Start the local server:
   - Click "Local Server" tab
   - Click "Start Server"
   - Default endpoint: `http://localhost:1234/v1/`
5. In Mantella configuration:
   - Set **LLM Service URL**: `http://localhost:1234/v1/`
   - Set **Model Name**: `gemma-2-4b-it` (or your chosen model)
   - Leave **API Key** blank for local models

### Option B: Ollama (Lighter & Faster)

**Pros**: Command-line based, very lightweight, fast startup

1. Install Ollama from [ollama.com](https://ollama.com)
2. Open Command Prompt (CMD) or PowerShell
3. Pull a model:
   ```bash
   ollama pull gemma2:4b
   ```
4. Ollama server runs automatically on `http://localhost:11434`
5. In Mantella configuration:
   - Set **LLM Service URL**: `http://localhost:11434/v1/`
   - Set **Model Name**: `gemma2:4b`
   - Leave **API Key** blank

### Option C: OpenRouter (Cloud-Based Fallback)

**Pros**: No local GPU needed, access to premium models
**Cons**: Requires API key, costs money per request

1. Sign up at [OpenRouter.ai](https://openrouter.ai)
2. Generate an API key from your dashboard
3. In Mantella configuration:
   - Set **LLM Service URL**: `https://openrouter.ai/api/v1/`
   - Set **Model Name**: `meta-llama/llama-3.1-8b-instruct` (or any supported model)
   - Set **API Key**: (paste your OpenRouter key)

---

## TTS (Text-to-Speech) Setup

Choose **ONE** of the following:

### Option A: xTTS (Best Quality, Free)

**Pros**: High-quality voices, free, clones voices well
**Cons**: Slower than alternatives, requires Python setup

1. Download **xTTS server** from [Mantella GitHub](https://github.com/art-from-the-machine/Mantella)
2. Follow the Python installation guide (requires Python 3.10+)
3. Run the xTTS server **before starting Skyrim**:
   ```bash
   python xtts_server.py
   ```
4. In Mantella:
   - Set **TTS Service**: `xTTS`
   - Default port: `8020`

### Option B: xVASynth (Lightweight, Skyrim-Native)

**Pros**: Very fast, designed for Skyrim voices, no Python needed
**Cons**: Lower quality than xTTS

1. Download [xVASynth](https://www.nexusmods.com/skyrimspecialedition/mods/44184)
2. Install and launch xVASynth
3. In Mantella:
   - Set **TTS Service**: `xVASynth`
   - Default port: `8008`

### Option C: ElevenLabs (Premium Cloud TTS)

**Pros**: Best voice quality available
**Cons**: Requires paid API, costs per character

1. Sign up at [ElevenLabs.io](https://elevenlabs.io)
2. Get your API key
3. In Mantella:
   - Set **TTS Service**: `ElevenLabs`
   - Set **API Key**: (your ElevenLabs key)

---

## First Launch & Testing

### Launch Sequence

1. **Start your LLM backend** (LM Studio, Ollama, etc.)
2. **Start your TTS service** (xTTS, xVASynth, etc.)
3. **Launch Mantella.exe** (leave it running)
4. **Start Skyrim via SKSE** through Mod Organizer 2

### In-Game Test

1. Load any save (new game NOT required)
2. Check the Mantella.exe console window:
   - Should say: `Running with local language model`
   - Should show active connections
3. Approach any NPC
4. Initiate dialogue
5. **Expected behavior**:
   - NPC responds with AI-generated text
   - Voice plays after 2-5 seconds
   - Subtitles display correctly

---

## Troubleshooting

### Issue: NPCs are silent
**Fixes**:
- Install **Fuz Ro D-oh** (required for subtitle fallback)
- Check Mantella.exe console for errors
- Verify TTS service is running
- Check voice folder paths in Mantella config

### Issue: No MCM menu for Mantella
**Fixes**:
- Ensure SKSE is installed correctly
- Check if Mantella.esp is enabled in load order
- Try loading a different save

### Issue: Long pauses before NPC speaks
**Causes**:
- Local LLM model too large for your hardware
- Skyrim installed on HDD instead of SSD
- Too many AI followers active at once

**Fixes**:
- Switch to a smaller model (4B parameter or less)
- Move Skyrim to SSD
- Limit to 2-3 AI followers max

### Issue: Mantella.exe says "Connection failed"
**Fixes**:
- Verify LLM service is running (check http://localhost:1234 or :11434)
- Check Windows Firewall isn't blocking local connections
- Restart LLM service

### Issue: Broken/robotic voices
**Fixes**:
- Update xTTS or xVASynth to latest version
- Clear Mantella voice cache folder
- Try different TTS service

---

## Performance Optimization

### For Low-End Systems:
- Use **Ollama** with `gemma2:2b` (smallest stable model)
- Use **xVASynth** for TTS (fastest)
- Limit to 1 AI follower
- Disable Papyrus logging

### For High-End Systems:
- Use **LM Studio** with `llama-3.1-8b-instruct`
- Use **xTTS** for best voice quality
- Can handle 3-4 AI followers

---

## Configuration Files

Mantella config is stored in:
```
C:\Users\<YourName>\AppData\Local\Mantella\config.json
```

Key settings:
```json
{
  "llm_api": "http://localhost:1234/v1/",
  "llm_model": "gemma-2-4b-it",
  "tts_service": "xTTS",
  "tts_port": "8020",
  "language": "en"
}
```

---

## Next Steps

Once Mantella is working:
1. Add **AI Voice Packs** for specific followers (Serana, Lydia, etc.)
2. Configure **personalities** in `/mantella/profiles/`
3. Install **CHIM** or **MinAI** for advanced AI behaviors (optional)
4. Set up **automated backups** using `/scripts/backup_ai_pack.ps1`

---

## Support & Resources

- [Mantella Nexus Page](https://www.nexusmods.com/skyrimspecialedition/mods/98631)
- [Mantella Discord](https://discord.gg/Q4BJAdtGUE)
- [GitHub Issues](https://github.com/art-from-the-machine/Mantella/issues)
- [Mantella Documentation](https://github.com/art-from-the-machine/Mantella/wiki)

---

**Installation Status**: ✅ Mantella Core Setup Complete
**Next**: Add follower profiles and voice packs
