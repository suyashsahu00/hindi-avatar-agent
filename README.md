# cd C:\Users\suyas\Downloads\CODING(1)\hindi-avatar-agent\hindi-avatar-agent

uv run src\agent.py dev

**cd**"C:\Users\suyas\Downloads\CODING(1)\hindi-avatar-agent\dr-sydney-frontend"
**npm** run dev

# Hindi Avatar Agent 🇮🇳

A Hindi-speaking, emotionally-expressive AI avatar agent built on LiveKit Agents.

| Layer                    | Provider                  | Detail                                                      |
| ------------------------ | ------------------------- | ----------------------------------------------------------- |
| **Avatar**         | Keyframe Labs             | `cosmo_persona-1.5-live` — lip-sync + emotion control    |
| **STT**            | Deepgram Nova-3           | `nova-3` model, `language=hi`                           |
| **LLM**            | OpenAI                    | `gpt-4.1-mini`                                            |
| **TTS**            | ElevenLabs                | `eleven_multilingual_v2` (or Azure `hi-IN-SwaraNeural`) |
| **VAD**            | Silero                    | —                                                          |
| **Turn detection** | LiveKit MultilingualModel | Hindi-aware                                                 |

---

## Prerequisites

- Python >= 3.10
- [`uv`](https://docs.astral.sh/uv/getting-started/installation/) package manager
- [`lk` CLI](https://docs.livekit.io/reference/developer-tools/livekit-cli/) (optional, for cloud deploy)
- Accounts & API keys for: LiveKit Cloud, Keyframe Labs, OpenAI, Deepgram, ElevenLabs

---

## Setup

### 1. Clone / open the project

```bash
cd hindi-avatar-agent
```

### 2. Fill in your API keys

Edit `.env.local` and replace every placeholder value:

```bash
LIVEKIT_URL=wss://your-project.livekit.cloud
LIVEKIT_API_KEY=...
LIVEKIT_API_SECRET=...
KEYFRAME_API_KEY=...
OPENAI_API_KEY=...
ELEVENLABS_API_KEY=...
DEEPGRAM_API_KEY=...
```

### 3. Install dependencies

```bash
uv sync
```

### 4. Download VAD / turn-detection model files

```bash
uv run src/agent.py download-files
```

---

## Running the agent

### Development mode (auto-reload, connects to LiveKit Cloud)

```bash
uv run src/agent.py dev
```

### Production mode

```bash
uv run src/agent.py start
```

### Console mode (terminal-only, no cloud needed)

```bash
uv run src/agent.py console
```

---

## Testing in the browser

1. Start the agent in `dev` mode (above).
2. Open the [LiveKit Agents Playground](https://agents-playground.livekit.io/).
3. Enter your LiveKit URL + key/secret, set **Agent name** to `hindi-avatar-agent`, and click **Connect**.
4. Speak in Hindi — the avatar will respond with lip-sync and emotion.

---

## Switching to Azure TTS (dedicated Hindi voice)

1. Add your Azure keys to `.env.local`:
   ```
   AZURE_SPEECH_KEY=...
   AZURE_SPEECH_REGION=eastus
   ```
2. In `pyproject.toml`, uncomment `livekit-plugins-azure`.
3. In `src/agent.py`, comment out the `elevenlabs.TTS(...)` block and uncomment the `azure.TTS(...)` block.
4. Run `uv sync` again.

---

## Emotion control

The LLM automatically calls `set_emotion` to change the avatar's expression:

| Emotion     | When used                          |
| ----------- | ---------------------------------- |
| `neutral` | Default / factual answers          |
| `happy`   | Good news, greetings, achievements |
| `sad`     | Condolences, bad news              |
| `angry`   | Frustration, criticism             |

You can also trigger emotions programmatically from your own code:

```python
await avatar.set_emotion("happy")
```

> **Note**: Emotion control requires a `persona-1.5-live` model (already configured).

---

## Project structure

```
hindi-avatar-agent/
├── src/
│   ├── agent.py       # AgentServer, session wiring, HindiAvatarAgent
│   └── prompts.py     # Hindi instructions & greeting
├── .env.local         # API keys (never commit)
├── .gitignore
├── pyproject.toml     # uv/hatch project config
└── README.md
```

---

## Useful links

- [LiveKit Agents docs](https://docs.livekit.io/agents/)
- [Keyframe avatar plugin](https://docs.livekit.io/agents/models/avatar/plugins/keyframe/)
- [Deepgram Nova-3 STT](https://docs.livekit.io/agents/models/stt/)
- [ElevenLabs TTS](https://docs.livekit.io/agents/models/tts/)
- [Keyframe platform (persona browser)](https://platform.keyframelabs.com)
