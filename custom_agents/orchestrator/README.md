# Gemini + GPT AI Orchestrator (Powered by Omnigent)

This orchestrator coordinates between **Google Gemini** and **OpenAI GPT** using [Omnigent](https://github.com/omnigent-ai/omnigent).

---

## Architecture

- **Orchestrator Brain (`orchestrator/config.yaml`)**:
  - Plans tasks and decomposes goals into sub-tasks.
  - Delegates to specialized sub-agents based on model strengths.
  - Conducts cross-model reviews (e.g. Gemini implements, GPT reviews, or vice versa).
- **Sub-agents (`orchestrator/agents/`)**:
  - `gemini`: Driven by the Google Antigravity SDK (`antigravity` harness). Best for deep architectural analysis, large-context reading, repository exploration, and creative solutions.
  - `gpt`: Driven by Codex (`codex` harness). Best for precision implementation, refactoring, test execution, and cross-model code review.

---

## Authentication & Credentials

### 1. OpenAI / GPT
- **Current status:** Already authenticated! Omnigent detects your active Codex subscription (`~/.codex/auth.json`).
- *(Optional)* You can also export `OPENAI_API_KEY="sk-..."` in your environment or `.env` if you prefer direct API usage.

### 2. Google Gemini
- If you have a Gemini API key:
  ```bash
  export GEMINI_API_KEY="AIzaSy..."
  ```
  *(Or add it to your `~/.zshrc` / project `.env`)*.
- Verify that both credentials are recognized at any time:
  ```bash
  omnigent config list
  ```

---

## How to Run

### 1. Launch the Multi-Model Orchestrator
From the repository root:
```bash
omnigent run orchestrator
```
*(Or use `omni run orchestrator`)*

When started, Omnigent:
1. Spawns the orchestrator session in your terminal.
2. Automatically launches the local web UI at `http://localhost:6767` with live dual-agent streaming, sub-agent task tracking, and terminal panels.

### 2. Launch Standalone Agents
To run directly on a single model without the multi-agent supervisor:
```bash
# Direct Gemini Agent:
omnigent run orchestrator/gemini_agent.yaml

# Direct GPT Agent:
omnigent run orchestrator/gpt_agent.yaml
```

### 3. Built-in Bundles
You can also run the built-in orchestrators shipped with Omnigent:
```bash
# Polly (Full multi-agent coding orchestrator across all available CLIs):
omnigent polly

# Debby (Two-headed debate partner comparing model perspectives):
omnigent debby
```
