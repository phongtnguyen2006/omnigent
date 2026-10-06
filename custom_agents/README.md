# 🎯 Custom Agents Directory

This directory is **strictly for your AI agent definitions, prompts, models, and workflows**. 
You **do not** need to touch the core Python framework code in the `omnigent/` directory to customize your agents.

---

## 📂 Where to Make Changes

| What You Want to Change | File to Edit | Key Fields to Modify |
| :--- | :--- | :--- |
| 🧠 **Orchestrator Behavior & Rules** | [`orchestrator/config.yaml`](./orchestrator/config.yaml) | `prompt:` (how work is planned, split, and reviewed), `executor:` |
| 🔷 **Gemini Sub-Agent Behavior** | [`orchestrator/agents/gemini/config.yaml`](./orchestrator/agents/gemini/config.yaml) | `prompt:` (instructions for Gemini), `model:` (e.g. `gemini-3.5-flash`, `gemini-1.5-pro`) |
| 🟢 **GPT Sub-Agent Behavior** | [`orchestrator/agents/gpt/config.yaml`](./orchestrator/agents/gpt/config.yaml) | `prompt:` (instructions for GPT), `model:` (e.g. `gpt-4o`, `gpt-4o-mini`, `o1`) |
| ⚡ **Standalone Gemini Agent** | [`orchestrator/gemini_agent.yaml`](./orchestrator/gemini_agent.yaml) | Single-file agent running purely on Gemini without delegation |
| ⚡ **Standalone GPT Agent** | [`orchestrator/gpt_agent.yaml`](./orchestrator/gpt_agent.yaml) | Single-file agent running purely on GPT without delegation |

---

## 🛠️ Cheat Sheet: Common Changes

### 1. Changing an Agent's System Prompt
Open the corresponding `config.yaml` file and edit the multiline `prompt:` string:
```yaml
prompt: |
  You are an expert Python backend developer.
  Always write unit tests with pytest for every new function.
  Follow PEP 8 styling strictly.
```

### 2. Changing the Underlying Model
Under `executor:`, specify or change the `model:` field:
```yaml
executor:
  type: omnigent
  config:
    harness: antigravity       # Harness (antigravity = Gemini, codex = GPT)
    model: gemini-3.5-flash    # Exact model identifier
```

### 3. Adjusting Reasoning Effort (o-series / Thinking models)
Add `reasoning_effort: low | medium | high | xhigh`:
```yaml
executor:
  type: omnigent
  config:
    harness: codex
    reasoning_effort: high
```

---

## 🚀 How to Run Your Agents

From the repository root (`~/VSCodeProjects/omnigent`):

```bash
# 1. Run the Multi-Model Orchestrator (Gemini + GPT collaborating):
omnigent run custom_agents/orchestrator

# 2. Run the Standalone Gemini Agent:
omnigent run custom_agents/orchestrator/gemini_agent.yaml

# 3. Run the Standalone GPT Agent:
omnigent run custom_agents/orchestrator/gpt_agent.yaml
```

*When any agent session starts, you can also view it live in your browser at **`http://localhost:6767`**.*
