# transcribe-meeting

> **Private, offline AI meeting transcription for macOS on Apple Silicon (M1/M2/M3/M4).**  
> Powered by Apple MLX and OpenAI Whisper. Zero cloud transmission. Self-bootstrapping setup.

---

## Features

- **100% Offline & Private:** Audio and transcripts never leave your machine. No cloud APIs, no external telemetry.
- **Apple Silicon Hardware Acceleration:** Uses Apple's MLX framework to run Whisper directly on Apple GPU and Neural Engine.
- **Zero-Friction Bootstrap:** Automatically detects if `uv` is installed, and installs it via Homebrew if needed.
- **Lossless Audio Extraction:** Accepts video files (`.mp4`, `.mov`, `.mkv`, etc.) and automatically remuxes the audio into `.m4a` without re-encoding quality loss.
- **Speaker & Turn Detection:** Detects conversational pauses (>1.2s cadence) to cleanly separate speaker turns.
- **Multi-Format Output:**
  - `_transcript.md` — Formatted specifically for LLM ingestion with turn headers, blockquotes, and timestamps.
  - `_transcript.json` — Complete structured segment data with millisecond timestamps.
  - `_transcript.txt` — Clean, continuous full-text transcript.

---

## Requirements

- **Operating System:** macOS (macOS 13 Ventura or newer recommended)
- **Architecture:** Apple Silicon (M1, M2, M3, M4)
- **Homebrew:** Optional, but recommended (the script will auto-install `uv` via Homebrew if `uv` is not present).

---

## Installation

### 1. Clone the repository
```bash
git clone https://github.com/<your-username>/transcribe-meeting.git
cd transcribe-meeting
```

### 2. Make it executable and add to your PATH
```bash
chmod +x transcribe-meeting
mkdir -p ~/.local/bin
cp transcribe-meeting ~/.local/bin/
```

Ensure `~/.local/bin` is in your shell `PATH` (typically in `~/.zshrc`):
```bash
export PATH="$HOME/.local/bin:$PATH"
```

---

## Usage

Transcribe any video or audio file with a single command:

```bash
transcribe-meeting "path/to/meeting_recording.mp4"
```

The script will automatically:
1. Ensure `uv` is present (installing via Homebrew if needed).
2. Extract the audio track into `.m4a` if a video file was supplied.
3. Transcribe using `whisper-large-v3-turbo` with Apple Silicon GPU acceleration.
4. Output `.md`, `.json`, and `.txt` files directly alongside the source file.

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

```bash
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
