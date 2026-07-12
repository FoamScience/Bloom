# Installation & Usage Guide

<p align="right"><a href="./GUIDE.zh.md">简体中文</a></p>

This guide helps you set up Bloom One-vs-One Study from scratch and start your first 1-on-1 AI tutoring session.

---

## Prerequisites

You need:

1. **Claude Code** (Anthropic's official CLI tool)
2. **A terminal** (macOS Terminal / iTerm2 / Windows Terminal / whatever you prefer)
3. **A text editor** (VS Code, Cursor, etc., for reading and annotating documents)

### Installing Claude Code

If you haven't installed Claude Code yet:

```bash
npm install -g @anthropic-ai/claude-code
```

After installation, run `claude` to confirm it launches properly. First run requires logging in to your Anthropic account.

> If you're unsure what Claude Code is: it's a CLI tool that lets you chat with Claude in your terminal, and Claude can directly read and write your local files. This is the foundation of how this system works.

---

## Setup

### Step 1: Clone the Repository

```bash
git clone https://github.com/Li-Evan/Bloom-one-vs-one-study.git
cd Bloom-one-vs-one-study
```

### Step 2: Install the Tutor Skill

Install the bundled tutor skill into this clone's local Claude Code skills directory:

```bash
mkdir -p .claude/skills
cp -R skills/bloom-tutor .claude/skills/
```

This keeps the public tutoring protocol in `skills/bloom-tutor/` instead of relying on local agent instruction files.

### Step 3: Launch Claude Code

Start Claude Code in the repository directory:

```bash
claude
```

Ask Claude to use the `bloom-tutor` skill when starting or continuing a course.

### Step 4: Start Your First Topic

In the Claude Code conversation, type:

```
Create a new folder and help me learn [your topic]
```

For example:

```
Create a new folder and help me learn Python decorators
```

```
Create a new folder and help me learn game theory basics
```

```
Create a new folder and help me learn personal income tax
```

On a new topic the tutor first asks **one short preference round** (at most 4 questions: your goal, relevant background, explanation style, scope) — the answers are stored as "Learner Notes" in the syllabus and every lesson is fitted to them. It only asks what your request didn't already answer.

Then Claude immediately generates:
- `syllabus.md` — course syllabus (defines all abilities you'll master, with a progress bar and a module dependency map)
- `01.md` — your first lesson document

Two knobs you can set in your first message (both optional):

- **Depth** — `simple` / `standard` (default) / `deep`
- **Length** — standard (~3-5 modules) or **extended** (~12-20 modules grouped into Parts): say *"marathon course on X"* or *"extended course on X"*

You can also ground a course in your own materials: *"learn X from ./books"* scans a folder of PDFs, builds a `sources.md` manifest, and cites your books throughout the course.

**Setup complete.** Here's how to use it.

---

## Usage Flow

### 1. Read the Document

Open the generated `.md` file in your text editor. Each document contains:

- **Prerequisites / Difficulty / Estimated reading time**
- **Main content** (knowledge with bold annotations, examples, and ⚠️ common-misconception boxes; every Part includes at least one 🌍 real-world artifact, not just textbook examples)
- **Thought questions** (2–3 questions, no answers given; from lesson 03 one is a 🔄 spiral question recalling earlier material)
- **🎯 Practice arena** (for hands-on skills: a generated exercise file under `practice/` with concrete tasks you do in the real tool — optional but recommended)
- **Feedback section** (where you write your feedback)

### 2. Annotate Your Confusions

While reading, write the following **anywhere you feel confused**:

```
???[Why use recursion here instead of a loop?]
```

Or use full-width question marks:

```
???[What's the intuitive meaning of this formula?]
```

You can place annotations anywhere in the text, as many as you want. These annotations are the most authentic snapshot of your thinking, and the tutor prioritizes them.

### 3. Answer Thought Questions & Write Feedback

At the bottom of the document in the "Your Feedback" section, write:

- Your answers to the thought questions (try to reason through them yourself — wrong answers are fine), each with a **confidence rating (1–5)**. Confident-but-wrong answers get flagged 🚩 and re-taught first — that's the point, so rate honestly
- Your insights, confusions, or topics you'd like the next lesson to dive deeper into
- How the 🎯 practice-arena tasks went, if you did them
- Anything else you want to say

### 4. Tell the Tutor You've Finished Reading

Go back to the Claude Code terminal and say:

```
I've finished reading
```

The tutor will:
1. Read all your annotations and feedback
2. Possibly ask you 1–2 key questions (max 2 rounds, no endless grilling)
3. Generate the next document

The next document's opening will include:
- **Thought question review** (evaluates each of your answers, provides correct answers)
- **??? responses** (addresses every confusion you annotated)
- **New content** (tailored to your understanding level)

### 5. Repeat Until Course Completion

In **extended courses**, each Part ends with a **part evaluation** ("mid-boss"): a Part Challenge over that Part's items plus a **Feynman gate** — you explain the Part's core in plain language, and it's graded (with a Part rank) at the start of the next lesson.

When all mastery items in the syllabus are covered, the system automatically generates the **final evaluation article** (no new content): a Final Challenge spanning the whole syllabus — and in extended courses a **capstone mini-project** exercising every Part. After you submit your answers, the system grades them, awards a **course rank (S/A/B/C)**, and auto-generates:

- `summary.md` — complete course summary with a completion certificate and your rank
- `cheatsheet.md` — a dense quick reference built for lookup-while-doing

Completed topics get **spaced flash reviews**: about a week later (then 30, then 90 days) the tutor offers a quick 3-question review to lock the material in. Always optional.

---

## Recording Summary Material

During your learning, if you encounter a particularly important insight you want in the final summary, annotate it:

```
#summary:[The essence of option pricing is replication — constructing a portfolio of known-price assets that reproduces the same cash flows]
```

The `#`-less format also works:

```
summary:[This analogy is brilliant — Nash equilibrium in game theory is like a traffic jam — no one can benefit by unilaterally changing routes]
```

These materials are automatically collected and integrated into `summary.md`.

---

## Advanced Usage

### Nested Directories

Topics can be organized by category:

```
Create a new folder under CFA, help me learn fixed income
```

This creates a `CFA/fixed-income/` directory.

### Parallel Topics

You can study multiple topics simultaneously. When entering Claude Code, tell the tutor which topic you'd like to continue:

```
I want to continue studying Python decorators, I've finished reading 02.md
```

Each course keeps its own Learner Notes — preferences never leak between unrelated topics.

### Side Quests

If a `???` annotation shows strong curiosity about something outside the syllabus, the tutor may offer an optional **side quest** (`sq-01.md` …) — a one-off detour article. Side quests never block or affect course progress, and you can always decline.

### Source-Grounded Courses

Point the tutor at your own library:

```
Learn statistical mechanics from ./books
```

It skims the tables of contents, writes a `sources.md` manifest (which chapters map to which modules, and what it left out — overridable), then teaches *through your books*: real citations (book, chapter, page), further-reading pointers per lesson, and an "Out of Scope" section that honestly lists what your corpus doesn't cover.

### Slash Commands

| Command | Action |
|---------|--------|
| `/organize-learning` | Scan all topics, log new documents to the learning journal |
| `/view-learning-log` | View historical learning records (newest first) |

---

## FAQ

### Q: Does it cost money?

The system itself is completely free and open-source. You need Claude Code access (requires an Anthropic account).

### Q: What topics are supported?

Anything you want to learn — programming, finance, philosophy, psychology, math, history... no limits.

### Q: Can I generate multiple documents at once?

No. This is a core design principle. The essence of 1-on-1 tutoring is that **every step adjusts based on your feedback**. Batch generation would break this feedback loop.

### Q: Where is my learning data stored?

Entirely on your local filesystem, in the cloned repository directory. No data is uploaded to the cloud. You can use Git to version-control your learning history.

### Q: Can I use other AI?

The bundled `skills/bloom-tutor` package is designed for Claude Code Skills. Other AI agents can work if you import the same instructions into their equivalent skill/instruction system, but results may vary.

### Q: What if I want to change direction mid-course?

Anytime. Write your desired direction change in the feedback section, and the tutor will adapt in the next lesson. Mastery items in the syllabus are the goals; the path is entirely flexible.

---

## Design Philosophy

This system is built on a simple belief:

> **The best learning isn't being lectured — it's being guided to discover.**

Traditional online courses are one-directional — pre-recorded videos won't pause to explain your confusions. ChatGPT-style Q&A is fragmented — you get answers, but no system.

This system aims to balance both: **systematic adaptive learning**. The syllabus ensures you stay on track, and the feedback loop ensures content always matches your level.

Bloom proved that 1-on-1 tutoring achieves +2σ. We believe a well-designed AI agent can approach this effect.
