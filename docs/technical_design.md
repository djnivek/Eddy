# Technical Design for Eddy

## High-Level System Vision

Eddy is built on a modular architecture that connects different functional components while remaining lightweight and extensible. The system is designed to be self-sufficient on a Raspberry Pi or similar device.

## Modules and Components

1. **Wake Word Detection Module**:

   - **Function**: Detect the wake phrase "Ok thing" with minimal resource consumption.
   - **Proposed Technology**:
     - [openWakeWord](https://github.com/dscripka/openWakeWord) (open source, flexible).
     - Alternatives: Picovoice Porcupine.

2. **Speech-to-Text & Understanding Module**:

   - **Function**: Convert captured audio into text and process intent.
   - **Proposed Technology**:
     - [Whisper](https://github.com/openai/whisper) (OpenAI, high precision).
     - Alternatives: 
       - Coqui Speech-to-Text (lighter)
       - ElevenLabs Flash (all-in-one solution, 75ms latency)
         - Pros: Extremely low latency, high accuracy
         - Cons: 
           - High cost
           - Limited flexibility for context injection
           - Complex API integration for advanced features
           - Cloud dependency

3. **AI Engine (LLM)**:

   - **Function**: Process queries and generate responses.
   - **Proposed Technology**:
     - [Llama 2](https://github.com/facebookresearch/llama) (Meta, optimized for local use).
     - Alternatives: Mistral, Qwen, Deepseek

4. **Text-to-Speech Module**:

   - **Function**: Synthesize natural audio responses.
   - **Proposed Technology**:
     - [Coqui TTS](https://github.com/coqui-ai/TTS) (open source).
     - Alternatives: 
       - VITS
       - ElevenLabs TTS (highest quality, but cloud-based)

5. **Integration Management**:

   - **Function**: Access calendars, local data, or external APIs.
   - **Examples**:
     - Google Calendar (REST API).
     - Weather information via OpenWeatherMap.

## Configuration Management

- **YAML-based Configuration**:
  - Dynamic switching between engines (Whisper, VITS, ...)
  - Environment-based settings
  - Secure API key management
  - Integration settings (Calendar, Weather)

## Module Communication

- **Wake Word ➔ Speech-to-Text ➔ LLM ➔ Text-to-Speech**:
  - Communication via **MQTT** for low-latency operations, or **HTTP REST** for simplicity.

## Development and Deployment

### Project Structure
```
eddy/
├── requirements/
│   ├── base.txt      # Core dependencies
│   ├── dev.txt       # Development tools
│   └── prod.txt      # Production dependencies
├── scripts/
│   └── setup.sh      # Installation script
├── config/
│   └── config.yaml   # Configuration
└── src/
    └── eddy/         # Source code
```

### Installation Methods

1. **Development Environment**:
   - Python virtual environment (recommended for development)
   - Requirements management by environment type
   - Development tools (testing, linting, formatting)

2. **Future Deployment Options**:
   - Docker containers (planned)
   - Automated installer
   - Raspberry Pi image

## Infrastructure and Resources

- **Recommended Hardware**:
  - Raspberry Pi 4 or higher (4-8 GB RAM for local AI models).
  - Medium-quality microphone.
  - Speakers for audio output.
- **Operating System**:
  - Raspberry Pi OS (Debian-based).
  - Use of **Docker** for service isolation.
- **Hardware Acceleration**:
  - Optional: Coral TPU or Nvidia Jetson for AI acceleration.

## Preliminary Steps

1. **Basic Prototype**:
   - Wake word ➔ Whisper transcription ➔ Static response via Text-to-Speech.
2. **Local AI Integration**:
   - Incorporate Llama 2 for dynamically generated responses.
3. **Contextual Integrations**:
   - Add access to calendar and weather queries.
4. **Optimizations**:
   - Reduce latency and optimize memory usage.