# Conversation Context & Setup History

This document captures the full context, technical research, and setup steps performed during our session so you can pick up immediately in this standalone project.

---

## 1. Initial Inquiry & Ecosystem Research

### User Request
> *"What is the best AI harness model orchestration GitHub open source so I can use an orchestrator with a Gemini and GPT account?"*

### Top Open-Source Frameworks Evaluated
1. **[Omnigent](https://github.com/omnigent-ai/omnigent) (Selected)**:
   - A unified **meta-harness** for AI coding agents that wraps models/engines like Claude Code, OpenAI Codex, Cursor Agent, and Google Antigravity.
   - Built-in multi-device session sync (terminal, browser UI at `http://localhost:6767`, desktop, mobile).
   - Uniform security guardrails and sandboxing.
   - Native support for delegating tasks between different vendor models and cross-reviewing diffs.
2. **[LiteLLM](https://github.com/BerriAI/litellm)**:
   - Best for API proxying, load-balancing, and fallbacks at the HTTP/SDK layer.
3. **[LangGraph](https://github.com/langchain-ai/langgraph)**:
   - Best for building cyclic graph-based agents and state machines in code.

---

## 2. Deep Dive: How Omnigent Works & Harness Selection

Omnigent interfaces with underlying agent runtimes via modular **harnesses**:
- `antigravity`: Google Antigravity Python SDK / Gemini native.
- `codex` / `openai-agents`: OpenAI Codex / GPT runtime.
- `claude-sdk`: Anthropic Claude Code engine.

### How Harnesses Are Chosen
1. **Declarative Specification (`config.yaml`)**: An agent explicitly declares its `executor.harness` (e.g. `antigravity` or `codex`).
2. **Sub-Agent Delegation**: An orchestrator spawns sub-agents tailored to each model's strengths:
   - **Gemini**: Large-context code exploration, architecture research, complex multi-file reasoning.
   - **GPT (Codex)**: Precision code generation, refactoring, test execution, and cross-model code review.
3. **Smart Routing**: An LLM prompt-classifier or AI gateway determines whether a task is trivial or complex, dynamically routing to the optimal harness.

---

## 3. Local Environment Setup Completed

During the session, the following system configurations were executed:

1. **Omnigent CLI Upgrade**:
   - Installed the `omnigent[antigravity]` extra using `uv tool install --upgrade "omnigent[antigravity]"`.
   - Added `google-antigravity` and `google-genai` libraries for native Gemini harness execution.
2. **Account Authentication**:
   - **OpenAI / GPT**: Active Codex CLI subscription located in `~/.codex/auth.json` is automatically recognized.
   - **Google Gemini**: Linked local Google OAuth credentials in `~/.gemini/oauth_creds.json` (also supports ambient `GEMINI_API_KEY`).
   - Run `omnigent config list` anytime to view active credential status.
3. **VS Code Setup**:
   - Moved `Visual Studio Code.app` into `/Applications/`.
   - Symlinked `/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code` to `~/.local/bin/code`.

---

## 4. Project Files in This Repository

- **[`config.yaml`](./config.yaml)**: The master orchestrator. Breaks down user requests, delegates to Gemini or GPT, and cross-reviews changes.
- **[`agents/gemini/config.yaml`](./agents/gemini/config.yaml)**: Google Gemini sub-agent prompt and tool settings.
- **[`agents/gpt/config.yaml`](./agents/gpt/config.yaml)**: OpenAI GPT sub-agent prompt and tool settings.
- **[`gemini_agent.yaml`](./gemini_agent.yaml)**: Standalone single-agent profile for Gemini.
- **[`gpt_agent.yaml`](./gpt_agent.yaml)**: Standalone single-agent profile for GPT.
- **[`README.md`](./README.md)**: Quickstart guide and command cheatsheet.

---

## 5. How to Run & Use

From this project directory (`~/VSCodeProjects/orchestrator`):

```bash
# 1. Start the Multi-Model Orchestrator:
omnigent run .
# or:
omni run .

# 2. Open the Web UI:
# Navigate to http://localhost:6767 in your browser

# 3. Open this project in VS Code:
code .
```
