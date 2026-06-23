# Awesome Manim Skills

A curated list of Agent Skills for creating animations with [Manim](https://www.manim.community/).

Agent Skills are modular packages that give AI coding agents (Claude Code, Gemini CLI, OpenCode, OpenAI Codex, Cursor, and others) instant expertise in Manim without manual prompting. Install any skill with a single line using the [skills CLI](https://skills.sh).

```bash
npx skills add <skill>
```

Browse the visual directory at [animo.video/skills](https://animo.video/skills).

---

## Contents

- [Compatibility matrix](#compatibility-matrix)
- [Skills](#skills)
  - [Planning](#planning)
  - [ManimCE](#manimce)
  - [ManimGL](#manimgl)
  - [Full Pipeline](#full-pipeline)
  - [Templates](#templates)
  - [Video Production](#video-production)
- [Recommended workflow](#recommended-workflow)
- [What are Agent Skills?](#what-are-agent-skills)
- [How to Install](#how-to-install)
- [Contributing](#contributing)

---

## Compatibility matrix

Each skill targets a specific Manim version, Python version, and set of AI agents. Use this before installing to avoid environment mismatches.

| Skill | Manim engine | Manim version | Python | Agent | System deps |
|---|---|---|---|---|---|
| Manim Composer | ManimCE or ManimGL | any | 3.7+ | Claude Code, Gemini CLI, OpenCode, Codex, any | FFmpeg, LaTeX |
| ManimCE Best Practices | ManimCE | any | 3.7+ | Claude Code, Gemini CLI, OpenCode, Codex, any | FFmpeg, LaTeX |
| ManimGL Best Practices | ManimGL | any | 3.7+ | Claude Code, Gemini CLI, OpenCode, Codex, any | FFmpeg, LaTeX, OpenGL |
| Manim Skill | ManimCE | latest | 3.7+ | Claude Code, Gemini CLI, OpenCode, Codex | FFmpeg, LaTeX, cairo |
| Manim Templates | ManimCE | any | 3.7+ | Claude Code, Gemini CLI, OpenCode, Codex, any | FFmpeg, LaTeX |
| Video Skills | ManimCE | any | 3.10+ | Claude Code, Gemini CLI, OpenCode, Codex, any | FFmpeg, LaTeX, edge-tts |

### Engine differences

| | ManimCE | ManimGL |
|---|---|---|
| Import | `from manim import *` | `from manimlib import *` |
| CLI | `manim` | `manimgl` |
| Default renderer | Cairo | OpenGL |
| Maintained by | Community | Grant Sanderson (3Blue1Brown) |
| Best for | Production, stable output | Interactive dev, 3D, rapid prototyping |
| Code compatible | No | No |

The two engines are **not interchangeable**. Code written for one will not run on the other without modifications. If you are unsure, start with ManimCE.

### Cairo vs OpenGL: what actually differs

ManimCE ships with two renderers: Cairo (default) and an experimental OpenGL mode (`--renderer=opengl`). ManimGL uses OpenGL exclusively. These are three distinct rendering paths with meaningfully different behavior.

| | Cairo (ManimCE default) | OpenGL (ManimCE experimental) | ManimGL OpenGL |
|---|---|---|---|
| Status | Stable, production-ready | Experimental, incomplete | Stable for 3b1b's workflow |
| Output | File via FFmpeg | File or live preview window | File or live preview window |
| CPU / GPU | CPU (software rasterization) | GPU | GPU |
| 3D support | Limited (ThreeDScene, no real depth) | Better, but depth ordering is buggy | Full, designed for 3D |
| LaTeX / MathTex | Full support | Full support | Full support |
| Z-ordering | Correct | Broken in some cases (issue #4721) | Correct |
| `Circumscribe` and some animations | Work | Had errors, partially fixed | Work |
| Live preview | No | Yes (`--preview`) | Yes |
| Recommended for | All production use and AI-generated code | Experimenting with live preview only | Interactive 3D development |

**The practical takeaway for AI agents:** generate code targeting the Cairo renderer. OpenGL in ManimCE has open bugs and unfinished internals (the codebase contains literal `TODO: Handle data locking / unlocking` in the play loop). Any skill that produces code expected to render reliably should default to Cairo, meaning no `--renderer=opengl` flag and no `OpenGLScene` subclass.

### System dependencies quick install

**macOS**
```bash
brew install cairo pkg-config ffmpeg mactex
```

**Ubuntu / Debian**
```bash
sudo apt install ffmpeg texlive-full libcairo2-dev
```

---

## Skills

### Planning

Use these before writing any code. They turn a vague idea into a structured scene-by-scene plan.

| Skill | Author | Description | Install |
|---|---|---|---|
| Manim Composer | adithya-s-k | Interviews you about topic, audience, and style, then outputs a full scenes.md ready for implementation | `npx skills add adithya-s-k/manim_skill/skills/manim-composer` |

### ManimCE

Skills for [Manim Community Edition](https://www.manim.community/), the actively maintained fork.

| Skill | Author | Description | Install |
|---|---|---|---|
| ManimCE Best Practices | adithya-s-k | Scene patterns, math objects, and smooth animations for 3Blue1Brown-style videos | `npx skills add adithya-s-k/manim_skill/skills/manimce-best-practices` |

### ManimGL

Skills for [ManimGL](https://github.com/3b1b/manim), Grant Sanderson's original fork used by 3Blue1Brown.

| Skill | Author | Description | Install |
|---|---|---|---|
| ManimGL Best Practices | adithya-s-k | High-quality mathematical animations with the original 3b1b engine | `npx skills add adithya-s-k/manim_skill/skills/manimgl-best-practices` |

### Full Pipeline

Skills that handle the full animation workflow: planning, coding, and rendering.

| Skill | Author | Description | Install |
|---|---|---|---|
| Manim Skill | Yusuke710 | Plans the scene, writes Manim code, and renders the video automatically inside Claude Code, Gemini CLI, OpenCode, or Codex | `npx skills add Yusuke710/manim-skill` |

### Templates

Pre-built scene templates for common educational and mathematical use cases.

| Skill | Author | Description | Install |
|---|---|---|---|
| Manim Templates | davila7 | Ready-to-use Manim scenes for AI agents, installable with a single line | `npx skills add davila7/claude-code-templates/manim` |

### Video Production

Skills that go beyond animation into narration, audio, and final video packaging.

| Skill | Author | Description | Install |
|---|---|---|---|
| Video Skills | lispking | Turns text prompts into full Manim videos with TTS narration, audio mixing, and cover art | `npx skills add lispking/video-skills` |

---

## Recommended workflow

Most Manim videos benefit from separating planning from implementation:

1. **Plan** with `manim-composer`: the agent researches the topic, asks targeted questions, and produces a `scenes.md` file specifying every scene.
2. **Implement** with `manimce-best-practices` or `manimgl-best-practices`: the agent has the scene plan and the full Manim API knowledge to write clean, renderable code.
3. **Package** with `video-skills` if you need narration, audio, and a cover on top.

Installing all three at once:

```bash
npx skills add adithya-s-k/manim_skill/skills/manim-composer
npx skills add adithya-s-k/manim_skill/skills/manimce-best-practices
npx skills add lispking/video-skills
```

---

## What are Agent Skills?

Agent Skills are an open standard for giving AI coding agents domain knowledge. A skill is a structured package (instructions, examples, context) that an agent loads before working on a specific library or task.

Once installed, your agent understands Manim conventions, common pitfalls, and best practices without you having to explain them in every prompt.

The standard is maintained at [agentskills.io](https://agentskills.io). Skills are published and discovered at [skills.sh](https://skills.sh).

---

## How to Install

The skills CLI ships with Node.js. No global install needed:

```bash
npx skills add <skill-install-command>
```

Example:

```bash
npx skills add adithya-s-k/manim_skill/skills/manimce-best-practices
```

The skill is immediately active in your agent's context.

---

## Contributing

To add a skill to this list:

1. The skill must be published on GitHub and follow the [Agent Skills format](https://agentskills.io).
2. The skill must be specifically for Manim (ManimCE, ManimGL, or a directly related tool).
3. Open a pull request adding a row to the appropriate table. Include: skill name, author, one-sentence description, and the install command.

Maintained by [Animo](https://animo.video).
