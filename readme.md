# SpeakEasy — Talk in Emoji, Answered by Muse Spark, in WebXR

> A safe-by-design VR/AR space where you communicate in universal emoji instead of raw voice, ask an AI fairy questions about the world in front of you, and interact with an autonomous child avatar who replies the way a kid in VR would—all in the browser on Meta Quest.

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

Open-mic voice chat is the default in spatial computing, but it creates two persistent challenges:

1. **Exclusion & Language Barriers:** Voice excludes people who can't or prefer not to speak—non-verbal users, the deaf and hard of hearing, players in noisy rooms, and anyone without a shared spoken language. 
2. **Safety in Shared Spaces:** Open audio is the primary vector for harassment, toxic language, and predatory behavior in social VR, particularly for younger players. Moderating live spatial audio after the fact is difficult, while virtual keyboards in headsets are slow and frustrating.

Emoji offer a universally recognized, language-agnostic visual vocabulary. **SpeakEasy decouples spoken intent from raw acoustic exposure**, turning emoji into a first-class, spatial communication layer:

- **Universal & expressive:** You can speak naturally or use your hands, but what is shared across space is a curated, symbolic message (12 symbols or fewer) accompanied by real-time avatar lip-sync, eye gaze, and gestures.
- **Safe by design:** Because communication is translated through bounded symbolic sequences with client- and model-level guardrails, toxic speech, slurs, and predatory language cannot reach another user's ears.
- **Accessible without typing:** No virtual keyboards or complex button chords—just natural voice translation and a tactile 3D wheel.

### Core interactions
1. **Pick emoji with your hands** on a spatial emoji wheel using tactile drag-and-drop.
2. **Speak naturally**, and Muse turns your words into a safe, concise emoji message grounded in what you're looking at.
3. **Ask Spark anything**, and an ambient fairy answers in emoji using real-time scene context.
4. **Interact with a kid avatar** who sees through its own eyes, reads your symbols, and replies safely in character as a child.

It runs entirely from a **single HTML file** with zero install and no build step. Open the URL in Meta Quest Browser and tap **Enter VR**.

---

## Features

### 🎡 3D emoji wheel
- Open the wheel with the index trigger on either controller (desktop: `L`). Drag the golden ball with your other hand toward an emoji on the rim, then release to pick.
- Multiple themed pages categorized by intent (VR Essentials, Feelings, Reactions, Hands & Signals, Body & Health, People, Jobs & Roles, Fairy Tale). Flip pages with the right thumbstick.
- Each emoji plays its own pitched note from a tuned pentatonic scale, giving communication an intuitive, musical feel.
- Closing the wheel sends your message floating above your head, visible to others and reflected in the virtual mirror.

### 🎙️ Speech → emoji (Muse Voice Transcribe + Muse Spark)
- Hold index + grip (desktop: `V`) and speak. **Muse Voice Transcribe** converts speech to text, and **Muse Spark** translates the text into an emoji sequence.
- **Multimodal scene grounding:** A lightweight snapshot of the room is attached as context only. Ambiguous terms (e.g., "that couch", "the blue planet") translate accurately without Spark narrating the background.
- **Strict safety filtering:** Lewd, hateful, or abusive phrases are intercepted and refused (`ANSWER: 🚫`), preventing harmful content from propagating into the space.
- Without an API key, SpeakEasy gracefully degrades to on-device speech recognition and a local phrase map.

### 🧚 Ask Spark — visual Q&A in emoji
- With the wheel closed, hold the trigger and speak a question. A ball of light forms while you talk, then a fairy emerges to answer in 12 or fewer emoji.
- **Multimodal awareness:** Spark inspects what you are currently looking at (in VR or AR passthrough) to answer questions like *"Who is that?"* or *"What is this object?"*
- **Live web search tool:** Capable of retrieving factual details on recent films, people, or products without conversational filler.
- Prompted strictly to answer the question asked rather than describing the surrounding room.

### 🧒 A kid avatar powered by Muse Spark
- An autonomous companion that wanders the platform, notices your presence, walks over, waves, and engages in friendly interaction.
- Captures snapshots from its own eye perspective, reads your emoji sequences, and replies in character as a young child in VR.
- Physicalized response: watch its own wheel appear, its procedural arm IK drag the golden ball to pick symbols, its facial expressions match each emoji, and notes play as it selects them.
- Keeps short-term conversation context to maintain natural, safe peer-like banter without exposing kids to real-world stranger audio.

### 📷 AR mode with room fit
- Switch between the virtual space and the real world via passthrough without leaving the headset.
- In AR, the system scans walls, ceilings, tables, couches, beds, doors, and windows. When returning to VR, the room **resizes to your real physical space** with stand-in collision geometry.
- Uses WebXR plane detection, mesh detection, and depth sensing for real-world per-pixel occlusion.

### 🙂 Expressive avatar
- Real-time procedural **mouth lip-sync** driven by microphone frequency analysis (formant tracking for wide smiles on "ee/s" and rounded lips on "oo"), automatic blinking, and focal eye convergence.
- Brows and mouth subtly mirror the emotion of the emoji you send.
- Supports WebXR **face tracking** (e.g., Quest Pro) and controller-free **hand tracking** with pinch triggers and gesture locomotion (beckon to walk forward, palm push to back up, air swipe to turn).

### 🌲 History
- Past Spark and kid answers are saved locally in the browser with the symbols or prompts that produced them.

---

## How it works

Hands / voice │ ├─ emoji wheel ───────────────────────────────► floating emoji
message │ ├─ speech ──► Muse Voice Transcribe ──► text ──► Muse Spark (safety
filtered) ──► emoji (≤12) │ │ (fallback: on-device speech recognition) │ ├─
hold-to-ask ► transcript + scene image ► Muse Spark (+ web search) ► fairy
answers in emoji │ └─ emoji to kid ► emoji + kid's-eye image ► Muse Spark (child
persona) ► kid avatar answers in emoji


- **Rendering:** Three.js (r160) via import maps, WebXR (`immersive-vr` and `immersive-ar`), and planar reflection shaders for the mirror.
- **WebXR features requested (optional):** `local-floor`, `bounded-floor`, `hand-tracking`, `camera-access`, `face-tracking`, `eye-tracking`, `plane-detection`, `mesh-detection`, `depth-sensing`.
- **Models:** Muse Spark (`muse-spark-1.3`) for translation, visual reasoning, and synthetic peer persona; Muse Voice Transcribe for audio input.
- **Safety architecture:** All system prompts constrain output to a maximum of 12 clean emoji, reject inappropriate requests with 🚫, and filter results through an explicit client-side blocklist.
- **Resilience:** Automatic request timeouts, text-only retry if image processing fails, and graceful fallback to local phrase mapping when offline.
- **Zero local persistence:** API keys and conversation logs remain strictly inside the client's `localStorage`.

---

## Running it

### On Meta Quest (recommended)
1. Host `vr.html` over **HTTPS** (GitHub Pages, Cloudflare Pages, Netlify, or Vercel). WebXR requires a secure context.
2. Open the URL inside **Meta Quest Browser**.
3. Tap **Enter VR**, or **📷 AR Mode** for passthrough.
4. Allow microphone access when prompted.

### On desktop (preview mode)
Open the page in any WebGL-compatible browser. Desktop controls:

| Action | Key / Input |
|---|---|
| Move / look | `WASD` / click-drag mouse |
| Open / close emoji wheel | `L` |
| Pick emoji | Mouse drag on golden ball |
| Ask Spark | Hold `L` (with wheel closed) |
| Speech → emoji | Hold `V` |
| Flip wheel page | `Q` / `E` or arrow keys |
| Jump | `Space` |
| Cancel voice capture | `Esc` |

For local testing: run `python3 -m http.server 8000` and navigate to `http://localhost:8000/vr.html`.

### API configuration
Open **⚙️ Settings** in VR (point at the gear and pull trigger, then tap ✏️ to type with the system keyboard) and paste your Meta API key. One key powers both Muse Voice Transcribe and Muse Spark.

Defaults can also be configured before loading via `window.MANUAL_CONFIG` in `vr.html`:

```js
window.MANUAL_CONFIG = {
  META_API_KEY: '',          // leave empty; do not commit keys
  PROXY_URL: '',             // optional proxy that injects the key server-side
  TRANSCRIBE_URL: '',
  SPARK_URL: '',
  SPARK_MODEL: 'muse-spark-1.3'
};

Note for Judges: The live submission deployment uses a Cloudflare Worker proxy
to handle requests out-of-the-box without requiring a manual API key. You may
also enter your own key in ⚙️ to hit endpoints directly.

Why it matters

  - Universal communication: Emoji transcend spoken languages, allowing users
    with different native tongues to communicate immediately without awkward
    translation delays.
  - Safe-by-design for youth: Replacing open-mic stranger audio with symbolic
    translation and structured AI peer interactions eliminates the primary
    vector for grooming, swearing, and verbal harassment in social VR.
  - Accessibility (AAC): Provides an expressive, spatial channel for non-verbal
    users or people who cannot speak in their physical environment.
  - Context-aware spatial AI: Instead of treating AI as an external chatbot,
    Spark and the avatar observe the 3D room alongside the user to anchor
    conversations in physical reality.

Known limitations

  - Passthrough access: Horizon OS restricts raw camera access in WebXR on
    certain firmware versions. When unavailable, Spark relies on spoken
    transcript context alone.
  - Hardware dependencies: Full face and eye tracking require supported hardware
    (e.g., Meta Quest Pro).
  - Symbolic brevity: Translating complex abstract concepts into 12 or fewer
    emoji can occasionally lead to ambiguous interpretations.
  - Network dependency: Muse endpoints require internet connectivity; offline
    mode is limited to common canned phrases.

Privacy

  - API keys and session history are stored exclusively in browser localStorage
    and can be wiped instantly via Clear key or Clear history.
  - Audio streams are captured only while holding the talk input. Scene
    snapshots are rendered only at the moment of an intentional Spark query or
    kid interaction.
  - No audio or video is ever written to disk or stored remotely.

Tech stack

HTML5 · Vanilla JavaScript (ES modules) · Three.js r160 · WebXR Device API · Web
Audio API · MediaDevices · Meta Muse Spark · Meta Muse Voice Transcribe ·
Cloudflare Workers

License

MIT. See the LICENSE file for details.

Acknowledgements

Built for the Meta Global AI Developer Hackathon 2026 using Meta Muse Spark,
Muse Voice Transcribe, Three.js, and the WebXR community.

