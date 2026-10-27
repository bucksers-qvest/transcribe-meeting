# transcribe-meeting

> **Private, offline AI meeting transcription for macOS on Apple Silicon (M1/M2/M3/M4).**  
> Powered by Apple MLX and OpenAI Whisper. Zero cloud transmission.

---

## Features

- **100% Offline & Private:** Audio and transcripts never leave your machine. No cloud APIs, no external telemetry.
- **Apple Silicon Hardware Acceleration:** Uses Apple's MLX framework to run Whisper directly on Apple GPU and Neural Engine.
- **Lossless Audio Extraction:** Accepts video files (`.mp4`, `.mov`, `.mkv`, etc.) and automatically remuxes the audio into `.m4a` without re-encoding quality loss.
- **Speaker & Turn Detection:** Detects conversational pauses (>1.2s cadence) to cleanly separate speaker turns.
- **Multi-Format Output:**
  - `_transcript.md` — Formatted specifically for LLM ingestion with turn headers, blockquotes, and timestamps.
  - `_transcript.json` — Complete structured segment data with millisecond timestamps.
  - `_transcript.txt` — Clean, continuous full-text transcript.

---

## Requirements

- **OS:** macOS 13 Ventura or newer
- **Architecture:** Apple Silicon (M1, M2, M3, M4)
- **Python:** 3.10 or newer
- **Python packages:** `mlx-whisper`, `av`, `numpy`

---

## Installation

### 1. Install Homebrew (if not already installed)

```zsh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 2. Install Python

```zsh
brew install python
```

### 3. Install Python dependencies

```zsh
pip3 install mlx-whisper av numpy
```

> **Note:** `mlx-whisper` pulls in Apple's MLX framework automatically. The Whisper model weights (~800 MB for the default `whisper-large-v3-turbo`) are downloaded from Hugging Face on first run and cached locally at `~/.cache/huggingface/`.

### 4. Clone the repository

```zsh
git clone https://github.com/<your-username>/transcribe-meeting.git
cd transcribe-meeting
```

### 5. Make the script executable

```zsh
chmod +x transcribe-meeting
```

### 6. Add to your PATH

```zsh
mkdir -p ~/.local/bin
cp transcribe-meeting ~/.local/bin/
```

Add `~/.local/bin` to your PATH in `~/.zshrc` if it isn't already:

```zsh
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

---

## Usage

Transcribe any video or audio file with a single command:

```zsh
transcribe-meeting "path/to/meeting_recording.mp4"
```

The script will automatically:
1. Extract the audio track into `.m4a` if a video file was supplied.
2. Transcribe using `whisper-large-v3-turbo` with Apple Silicon GPU acceleration.
3. Output `.md`, `.json`, and `.txt` files directly alongside the source file.

---

## Options & Flags

```console
$ transcribe-meeting -h
usage: transcribe-meeting [-h] [--model MODEL] [--pause PAUSE] [-o OUTPUT_DIR]
                          [--verbose]
                          input_file

positional arguments:
  input_file            Path to video (.mp4, .mov, etc.) or audio (.m4a, .wav,
                        etc.) file

options:
  -h, --help            show this help message and exit
  --model MODEL         HuggingFace MLX model identifier (default: mlx-
                        community/whisper-large-v3-turbo)
  --pause PAUSE         Silence threshold in seconds to detect change of
                        speaker / conversational turn (default: 1.2s)
  -o, --output-dir OUTPUT_DIR
                        Directory to save transcript files (defaults to same
                        folder as input file)
  --verbose             Print transcription segments in real time to the
                        terminal
```

### Examples

```zsh
# Stream live transcription segments to the terminal as they are processed
transcribe-meeting "presentation.mov" --verbose

# Fast draft using smaller model (whisper-base or whisper-small)
transcribe-meeting "interview.m4a" --model mlx-community/whisper-base

# Custom speaker pause sensitivity (e.g. 1.5 seconds) and custom output folder
transcribe-meeting "working_session.mp4" --pause 1.5 -o ~/Documents/Transcripts
```

---

## Security & Corporate Privacy

For an in-depth security analysis, data-flow diagram, and IT compliance audit, please refer to [SECURITY.md](SECURITY.md).

---

## License

MIT License. See [LICENSE](LICENSE) for details.
