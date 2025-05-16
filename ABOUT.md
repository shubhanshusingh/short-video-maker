# Short Video Maker

## Overview
Short Video Maker is a powerful open-source tool that automates the creation of short-form videos by combining:
- Text-to-speech conversion (Kokoro TTS)
- Automatic caption generation (Whisper)
- Background video integration (Pexels API)
- Background music with mood selection
- Professional video composition (Remotion)

## Technical Stack
- Built with TypeScript/Node.js
- Docker support with three variants:
  - Tiny (lightweight)
  - Normal (standard)
  - CUDA (GPU-accelerated)
- Web UI for browser-based video generation
- REST API and MCP server support

## System Requirements
- Internet connection
- Free Pexels API key
- Minimum 3GB RAM (4GB recommended)
- 2 vCPU
- 5GB disk space
- Supported on Ubuntu ≥ 22.04 and Mac OS

## Current Limitations
- English voiceover only
- Background videos sourced exclusively from Pexels

## Key Features
- Generate complete short videos from text prompts
- Text-to-speech conversion
- Automatic caption generation and styling
- Background video search and selection via Pexels
- Background music with genre/mood selection
- Serve as both REST API and Model Context Protocol (MCP) server

## How It Works
The tool takes simple text inputs and search terms, then:
1. Converts text to speech using Kokoro TTS
2. Generates accurate captions via Whisper
3. Finds relevant background videos from Pexels
4. Composes all elements with Remotion
5. Renders a professional-looking short video with perfectly timed captions

## Dependencies
| Dependency | Version | License | Purpose |
|------------|---------|---------|---------|
| [Remotion](https://remotion.dev/) | ^4.0.286 | Remotion License | Video composition and rendering |
| [Whisper CPP](https://github.com/ggml-org/whisper.cpp) | v1.5.5 | MIT | Speech-to-text for captions |
| [FFmpeg](https://ffmpeg.org/) | ^2.1.3 | LGPL/GPL | Audio/video manipulation |
| [Kokoro.js](https://www.npmjs.com/package/kokoro-js) | ^1.2.0 | MIT | Text-to-speech generation |
| [Pexels API](https://www.pexels.com/api/) | N/A | Pexels Terms | Background videos |

## Acknowledgments
- ❤️ [Remotion](https://remotion.dev/) for programmatic video generation
- ❤️ [Whisper](https://github.com/ggml-org/whisper.cpp) for speech-to-text
- ❤️ [Pexels](https://www.pexels.com/) for video content
- ❤️ [FFmpeg](https://ffmpeg.org/) for audio/video processing
- ❤️ [Kokoro](https://github.com/hexgrad/kokoro) for TTS 