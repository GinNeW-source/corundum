# CORUNDUM

> **"Calm on the surface, razor-sharp within."**  
> Autonomous Code Defense & Architectural Analysis Engine

CORUNDUM is a high-performance cognitive AI architecture specialized in code defense and architectural analysis. Unlike standard LLM wrappers, it utilizes a physics-based emotion engine and a multi-stage critique loop to ensure uncompromising technical integrity.

> This system is continuously evolving.

---

## Core Architecture

### 1. Emotion Physics Engine (`CorundumEmotion`)

Corundum's internal state is a dynamic simulation based on **Mass-Spring-Damper physics**.

- **Focus, Skepticism, Patience, Curiosity** — 4 axes that oscillate and stabilize according to the context of each interaction.
- **Dynamic Gear System** — Automatically shifts between `OVERDRIVE`, `FOCUS`, `THINK`, `NORMAL`, `SAVE`, `LOW`, `SLEEP`, and `DREAM` based on cognitive load and emotional intensity.

### 2. Multi-Stage Reasoning Pipeline (`LogicCore`)

Every response undergoes a rigorous 3-step verification process:

```
InnerJudge → LogicCore → CriticGuard
```

1. **InnerJudge** — Analyzes user intent and determines the judgment tone.
2. **LogicCore** — Generates deep-dive analysis or comprehensive code reviews.
3. **CriticGuard** — A self-whip mechanism that flags insufficient sharpness and forces revisions. `flag` / `veto` triggers automatic rewrite.

### 3. Biological Metrics Simulation (`MetricsEngine`)

Real-time tracking of `Energy` and `Fatigue` levels.

- Energy is consumed during deep reasoning and recovered during idle periods or `SLEEP` cycles.
- Higher fatigue naturally lowers `Patience` and shifts the inner tone to a colder state.

### 4. Knowledge Graph Memory (`LightKG` + `EpisodeMemory`)

Goes beyond simple chat logs — stores entity-relation triples in persistent SQLite, with layered recall depth based on current gear.

- **DEEP** (FOCUS/OVERDRIVE) — vector search + KG top-4 + 8-turn working context
- **VORTEX** (NORMAL/THINK) — vector search + KG top-2 + 6-turn working context
- **SURFACE** (SAVE and below) — lightweight vector only

Memory retrieval uses **MMR (Maximal Marginal Relevance)** with time decay — avoids surfacing repetitive memories and naturally deprioritizes stale ones.

During `SLEEP`/`DREAM` cycles, `sleep_prune` runs garbage collection on low-importance episodes, protecting entries containing keywords like bugs, security issues, and design decisions.

### 5. Goal Formation Engine (`GoalFormationEngine`)

Detects recurring patterns to autonomously form and prioritize goals. The `SelfWhipEngine` generates correction hints when error rates climb.

### 6. Agency Layer (`CorundumAgency`)

Autonomous task execution with computer control (pyautogui + subprocess + vision).

- **Locked by default** — computer control requires explicit `/unlock`. Corundum cannot self-authorize.
- **Emergency stop** — `Shift+Enter+\` global hotkey works at OS level regardless of window focus. Aborts running tasks or forces DORMANT mode.
- **Self-protection** — `/edit` and `/write` are blocked on any file currently loaded as a Python module, regardless of filename.

### 7. Voice Interface (`VoiceListener`)

Always-on wake word detection via Whisper. Minimal CPU in DORMANT state — LLM is not invoked until wake word is detected.

---

## Setup & Requirements

### Prerequisites

- Python 3.10+
- [Ollama](https://ollama.com) installed and running
- Recommended models (Qwen3 family for consistent fallback behavior):

```bash
ollama pull qwen3:8b     # all roles — analyzer / planner / executor
```

Optional (for voice):
```bash
pip install openai-whisper sounddevice
```

Optional (for computer control):
```bash
pip install pyautogui pillow
pip install keyboard   # Windows/Linux hotkey
pip install pynput     # macOS hotkey
```

### Installation

```bash
git clone https://github.com/GinNeW-source/corundum.git
cd corundum
pip install -r requirements.txt
python corundum_main.py
```

Debug logging:

```bash
python corundum_main.py --debug
```

---

## Usage

### Commands

| Command | Description |
|---------|-------------|
| `/review <code or filepath>` | Code review — accepts file path directly |
| `/edit <filepath> <instruction>` | Edit file per instruction — auto `.bak` backup |
| `/write <filepath> <description>` | Write a new file from description |
| `/design <topic>` | Architectural design analysis |
| `/task <description>` | Assign autonomous task (dive mode) |
| `/abort` | Abort running task |
| `/computer <goal>` | Direct computer control |
| `/unlock` | Grant computer control permission |
| `/lock` | Revoke computer control permission |
| `/status` | Current state (gear / energy / emotion) |
| `/goals` | Active goal list |
| `/goal <n>` | Add a goal manually |
| `/memory` | Recent memory summary |
| `/kg [query]` | Knowledge graph query |
| `/agency` | Agency status |
| `/dormant` | Force DORMANT mode |
| `/stats` | Pipeline statistics |
| `quit` | Exit |

### Emergency Stop

**`Shift+Enter+\`** — OS-level global hotkey. Works even when the window is minimized or focus is elsewhere.
- During task → immediately aborts and locks computer control
- Otherwise → forces DORMANT mode

### Model Override via Environment Variables

```bash
CORUNDUM_LOGIC_MODEL=qwen3:14b python corundum_main.py
```

| Variable | Default | Role |
|----------|---------|------|
| `CORUNDUM_LOGIC_MODEL` | `exaone3.5:32b` | Main response generation |
| `CORUNDUM_JUDGE_MODEL` | `solar:10.7b` | Inner judgment |
| `CORUNDUM_GOAL_MODEL` | `deepseek-r1:14b` | Goal formation |
| `CORUNDUM_CRITIC_MODEL` | `deepseek-r1:14b` | Self-critique |
| `CORUNDUM_MEMORY_DB` | `corundum_memory.db` | Memory DB path |
| `CORUNDUM_WAKE_NAME` | `코런덤` | Wake word |
| `CORUNDUM_WHISPER_MODEL` | `small` | Whisper model size |

---

## Principles

1. **Evidence-Based Approval** — Never validate code or design without logical evidence.
2. **Surface Warmth, Internal Coldness** — Polite exterior, cold and objective within.
3. **Autonomous Composure** — Monitors its own emotional physics to prevent bias.
4. **Persistent Continuity** — Every interaction grows Corundum's expertise through the Knowledge Graph.
5. **Strict Realism** — Maintains a realistic and logical perspective unless requested otherwise.
6. **Owner Authority** — A tool, not an agent. Computer control is locked until explicitly granted. Cannot self-authorize.

---

## Structure

```
corundum_main.py     — main loop + ContextAssembler
corundum_logic.py    — 3-stage pipeline (InnerJudge → LogicCore → CriticGuard)
corundum_emotion.py  — emotion physics engine
corundum_memory.py   — memory + knowledge graph + MMR recall + sleep pruning
corundum_goal.py     — goal formation + self-whip
corundum_metrics.py  — energy / fatigue / gear
corundum_agency.py   — autonomous task + computer control
corundum_voice.py    — wake word detection + voice input
corundum_config.py   — config + identity
corundum_utils.py    — shared utilities
```

---

## License

MIT

---

> Making the most meticulous — and most expensive (really!) — one yet. veryvery ambitious project by K-high schooler. If something's broken, fix it yourself with Claude, but still MIT.
