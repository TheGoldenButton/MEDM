# SpeakEasy — Talk in Emoji, Answered by Muse Spark, in WebXR

> A social VR/AR space where you communicate with emoji instead of words, ask an AI fairy questions about the world in front of you, and chat with a kid avatar who answers the way a child would, all in the browser on Meta Quest.

**Meta Global AI Developer Hackathon 2026 submission**

| | |
|---|---|
| **Track / Category** | Indie |
| **Team** | SpeakEasy |
| **Live demo** | https://thegoldenbutton.github.io/SpeakEasy/ |
| **Repo** | https://github.com/TheGoldenButton/SpeakEasy |
| **Meta technologies** | Muse Spark, Muse Voice Transcribe, Meta Quest Browser / WebXR |

---

## The idea

Voice chat is the default in VR, but it excludes people who can't or don't want to speak: players in a noisy room, people who are shy, deaf or hard of hearing, kids, and anyone without a shared language. Emoji are a nearly universal visual vocabulary, but typing them on a headset is miserable.

SpeakEasy makes emoji a first-class, spatial, spoken-feeling language in VR:

1. **Pick emoji with your hands** on a 3D emoji wheel (no keyboard).
2. **Speak instead**, and Muse turns your words into a short emoji message (12 symbols or fewer).
3. **Ask Spark anything**, and a fairy answers in emoji, using what you're looking at as context.
4. **Talk to a kid avatar** who reads your emoji, sees through its own eyes, and replies like a child in VR would.

It runs from a **single HTML file**, with no install and no build step. Open the URL in Meta Quest Browser and tap Enter VR.

## Features

### 🎡 3D emoji wheel
- Open the wheel with the index trigger on either controller (desktop: `L`). Drag the golden ball with your other hand toward an emoji on the rim, then release to pick.
- Many themed pages, each with its own set of emoji (for example VR Essentials, Feelings, Reactions, Hands & Signals, Body & Health, People, Jobs & Roles, Fairy Tale). Flip pages with the right thumbstick.
- Each emoji plays its own pitched note, so picking emoji sounds like playing a tiny instrument.
- Close the wheel to send. Your message floats above your head, and you can watch it in the mirror.

### 🎙️ Speech → emoji (Muse Voice Transcribe + Muse Spark)
- Hold index + grip (desktop: `V`) and speak. **Muse Voice Transcribe** converts speech to text, and **Muse Spark** translates the text into an emoji sequence.
- Scene context: a snapshot of the room is attached as context only, so ambiguous words are translated sensibly without Spark describing the scene.
- Without an API key, SpeakEasy falls back to the headset's built-in speech recognition and a phrase→emoji map.

### 🧚 Ask Spark — a fairy that answers in emoji
- With the wheel closed, hold the trigger and ask a question. A ball of light forms while you talk, then a fairy emerges and answers in 12 or fewer emoji.
- **Multimodal:** the model receives the image of what you are looking at (the VR room, or the real room in AR), so "who is that?" or "what is this movie?" can work.
- **Web search tool** for recent or specific things (films, news, people, products).
- Spark is prompted to answer the question asked, not describe the scene.

### 🧒 A kid avatar powered by Muse Spark
- The avatar wanders the platform, notices you, turns its head, walks over, waves, and drifts off again.
- It receives your emoji and what it can see from its own eyes, then answers in character as a young child.
- You can watch it use its own wheel: its arm drags the golden ball to each emoji at a different speed, its face makes each expression, and every pick plays a note.
- Conversation memory is kept in the chat so replies stay coherent.

### 📷 AR mode with room fit
- Switch between the virtual room and the real world without leaving the headset.
- In AR, the headset scans walls, ceiling height, tables, couches, beds, doors and windows. When you return to VR, the virtual room **resizes to your real space** with stand-ins for your furniture. Spark stays inside your real walls and walks around your furniture.
- Uses WebXR plane detection, mesh detection and depth sensing (per-pixel real-world occlusion).

### 🙂 Expressive avatar
- Microphone-driven **lip-sync** (wide smile on "ee/s", rounded lips on "oo"), blinking, and eyes that follow the wheel or fairy.
- Brows and smile react to the emoji you send.
- Uses WebXR **face tracking** when available (for example Quest Pro). Hand tracking is supported, with pinch gestures as trigger/grip and hands-only locomotion (beckon, push, swipe, flick).

### 🌲 History
- Every Spark and kid answer is saved locally with the symbols or question that produced it.

## How it works

```
 Hands / voice
      │
      ├─ emoji wheel ───────────────────────────────► floating emoji message
      │
      ├─ speech ──► Muse Voice Transcribe ──► text ──► Muse Spark ──► emoji (≤12)
      │                      │ (fallback: on-device speech recognition)
      │
      ├─ hold-to-ask ► transcript + scene image ► Muse Spark (+ web search) ► fairy answers in emoji
      │
      └─ emoji to kid ► emoji + kid's-eye image ► Muse Spark ► kid avatar answers in emoji
```

- **Rendering:** Three.js (r160) via an import map, WebXR (`immersive-vr` and `immersive-ar`), and a `Reflector` for the mirror.
- **WebXR features requested (optional):** `local-floor`, `bounded-floor`, `hand-tracking`, `camera-access`, `face-tracking`, `eye-tracking`, `plane-detection`, `mesh-detection`, `depth-sensing`.
- **Models:** Muse Spark (`muse-spark-1.3` by default) for translation, Q&A and the kid persona; Muse Voice Transcribe for speech-to-text.
- **Prompting:** three purpose-built system prompts (translate, answer, kid persona), each constrained to output a single parseable line of 12 or fewer emoji, with a structured `THINK:` / `ANSWER:` format for Q&A.
- **Safety:** all prompts instruct the model to refuse lewd or hateful content, answering with 🚫.
- **Resilience:** responses time out and retry, image-rejected requests retry text-only, and the app degrades to on-device speech and phrase maps if the API is unavailable.
- **Storage:** the API key, settings and history live only in the browser's `localStorage`.

## Running it

### On Meta Quest (recommended)
1. Host `vr.html` over **HTTPS**, for example GitHub Pages, Cloudflare Pages, Netlify, or Vercel. WebXR requires a secure context.
2. Open the URL in **Meta Quest Browser**.
3. Tap **Enter VR**, or **📷 AR Mode** for passthrough.
4. Allow microphone access when prompted.

### On desktop (preview, no headset)
Open the page in a modern browser. A desktop fallback is built in:

| Action | Key |
|---|---|
| Move / look | `WASD` / drag |
| Open or close emoji wheel | `L` |
| Pick emoji | mouse drag on the wheel |
| Ask Spark | hold `L` (wheel closed) |
| Speech → emoji | hold `V` |
| Flip wheel page | `Q` / `E` or arrow keys |
| Jump | `Space` |
| Cancel | `Esc` |

For local testing: `python3 -m http.server 8000`, then open `http://localhost:8000/vr.html` (localhost counts as a secure context for most features).

### API configuration
Open **⚙️ Settings** (in VR, point at it and pull the trigger, then tap ✏️ to type with the Meta keyboard; requires Meta Quest Browser 26.1+) and paste your Meta API key. One key is used for both Muse Voice Transcribe and Muse Spark. Endpoints and model name are editable.

Defaults can also be set before load via `window.MANUAL_CONFIG` in `vr.html`:

```js
window.MANUAL_CONFIG = {
  META_API_KEY: '',          // leave empty; do not commit real keys
  PROXY_URL: '',             // optional proxy that injects the key server-side
  TRANSCRIBE_URL: '',
  SPARK_URL: '',
  SPARK_MODEL: 'muse-spark-1.3'
};
```

> **Judges:** the submitted build can use a Cloudflare Worker proxy so the app works with zero setup and no key is ever shipped to the client. If you prefer, paste your own key in ⚙️ and the app will call Meta directly.

## Why it matters

- **Accessibility:** a communication channel for people who can't or don't want to use voice.
- **Language-agnostic:** emoji bridge languages; Spark turns any spoken language into a shared symbol message.
- **Safer social VR for kids:** the kid persona and the refusal rules explore friendly, low-stakes, non-verbal interaction.
- **Multimodal AI in space:** the model reasons about what you're looking at, inside VR or AR, not just text.

## Known limitations

- **Passthrough camera:** Horizon OS currently blocks raw passthrough camera frames in WebXR on some devices. When no frame is available, Spark answers from speech alone, and the app says so.
- Face tracking and eye tracking need hardware that exposes them (for example Quest Pro).
- Emoji meaning is culturally variable, and 12 symbols is a deliberately tight budget.
- Model answers can be wrong or ambiguous, and they are shown as-is.
- Requires a network connection for Muse APIs. The offline fallback is limited to common phrases.

## Privacy

- The API key and history are stored only in your browser. Clear them any time via Settings (**Clear key**) and History (**Clear history**).
- Audio is sent to Muse Voice Transcribe only while you hold the speak control. Scene or camera snapshots are sent to Muse Spark only when you ask Spark or message the kid avatar.
- The microphone and camera are never recorded to disk by the app.

## Project structure

```
vr.html     # entire app: UI, Three.js scene, WebXR, input, audio, Muse API clients
README.md
LICENSE     # MIT
```

## Tech stack

HTML5 · JavaScript (ES modules) · [Three.js](https://threejs.org) r160 · WebXR Device API · Web Audio · MediaDevices · Meta Muse Spark · Meta Muse Voice Transcribe · (optional) Cloudflare Workers proxy

## Roadmap

- Multiplayer rooms, so two people can talk entirely in emoji
- Custom emoji pages and saved phrases
- Spark-as-interpreter between two users who speak different languages
- On-device speech and emoji translation for offline use

## License

MIT. See the `LICENSE` file.

## Acknowledgements

Built for the Meta Global AI Developer Hackathon 2026 with Meta's Muse Spark and Muse Voice Transcribe, Three.js, and the WebXR community.
