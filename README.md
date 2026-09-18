<div align="center">

# 🧠 Building AI Agents

### LLM Calls → Tool Use → Memory → Reasoning → Multi-Agent Systems → Production

**A hands-on AI Agent Engineering Lab**

I'm using this repo to understand, build, break, and improve AI agents — from the smallest working agent loop to systems that plan, use tools, maintain state, collaborate, and run inside real workflows.

<br/>

[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](#)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](#)
[![AI Agents](https://img.shields.io/badge/AI_Agents-Engineering-blueviolet?style=for-the-badge)](#)
[![Status](https://img.shields.io/badge/Status-Active_Development-2ea44f?style=for-the-badge)](#)

**Build • Experiment • Evaluate • Document • Repeat**

<br/>

[🎯 What this is](#-what-this-is) •
[🧩 What I'm building](#-what-im-building) •
[🏗️ Architecture](#️-agent-architecture) •
[📚 Learning path](#-learning-path) •
[🗺️ Roadmap](#️-roadmap)

</div>

---

## 🎯 What this is

Most software systems follow instructions. AI agents do something different — they interpret a goal, decide what to do, use tools, observe results, adjust, and keep working toward that goal.

I want to understand that at the engineering level, not just the API-call level.

```text
                  ┌───────────────────────┐
                  │        User Goal      │
                  └───────────┬───────────┘
                              │
                              ▼
                  ┌───────────────────────┐
                  │   Context / State     │
                  │ Memory • History      │
                  │ Task • Constraints    │
                  └───────────┬───────────┘
                              │
                              ▼
                  ┌───────────────────────┐
                  │   Reasoning / Plan    │
                  └───────────┬───────────┘
                              │
                    ┌─────────┴─────────┐
                    ▼                   ▼
             ┌─────────────┐     ┌─────────────┐
             │    Tools    │     │   Agents    │
             │ APIs • DB   │     │ Sub-agents  │
             │ Web • Code  │     │ Specialists │
             └──────┬──────┘     └──────┬──────┘
                    │                   │
                    └─────────┬─────────┘
                              ▼
                  ┌───────────────────────┐
                  │ Observation / Result  │
                  └───────────┬───────────┘
                              │
                              ▼
                  ┌───────────────────────┐
                  │ Evaluate • Correct    │
                  │ Retry • Continue      │
                  └───────────┬───────────┘
                              │
                         ┌────▼────┐
                         │ Output  │
                         └─────────┘
```

The question running through every experiment here is: **what engineering decisions make an AI system more capable, reliable, and actually useful?**

---

## 🤖 What I'm building

The repo is organized as an evolving lab — implementations, experiments, notes, and real agent patterns, roughly in the order I'm learning them.

### 🧠 LLM Fundamentals

Understanding the raw model interface before wrapping it in abstractions — prompt design, structured output, message history, context windows, streaming, tool calling, model routing, and where models fail.

---

### 🛠️ Tool-Using Agents

Giving a language model the ability to interact with software: web search, APIs, databases, file systems, code execution, external services.

The core loop the experiments run on:

```text
Think → Choose Tool → Execute → Observe Result → Decide Next Action
```

---

### 🧩 RAG & Knowledge Systems

How agents work with information beyond their immediate context — document ingestion, chunking, embeddings, vector retrieval, metadata filtering, hybrid search, and how retrieval quality actually affects generation.

---

### 🧠 Memory & State

An agent that can't hold useful state struggles with anything beyond a single turn. Experiments cover conversation memory, short-term state, long-term memory, working memory, retrieval-based memory, and context compression.

---

### 🗺️ Planning & Reasoning

Moving from `Input → Answer` toward a full goal-plan-action-observe-revise loop. Topics include ReAct-style execution, task decomposition, reflection, self-correction, retry strategies, and long-horizon execution.

---

### 🤝 Multi-Agent Systems

When one agent isn't enough:

```text
                    ┌──────────────┐
                    │ Coordinator  │
                    └──────┬───────┘
                           │
          ┌────────────────┼────────────────┐
          ▼                ▼                ▼
   ┌────────────┐   ┌────────────┐   ┌────────────┐
   │ Researcher │   │  Coder     │   │  Analyst   │
   └────────────┘   └────────────┘   └────────────┘
          │                │                │
          └────────────────┼────────────────┘
                           ▼
                    ┌──────────────┐
                    │   Synthesis  │
                    └──────────────┘
```

Experiments will cover specialist agents, delegation, supervisor architectures, agent handoffs, parallel execution, and how failures propagate across agent boundaries.

---

## 🏗️ Agent Architecture

```text
┌─────────────────────────────────────────────────────────┐
│                       APPLICATION                        │
├─────────────────────────────────────────────────────────┤
│ User Interface / API / CLI / Automation                 │
├─────────────────────────────────────────────────────────┤
│                    AGENT ORCHESTRATION                   │
│                                                         │
│ Planning • Routing • Delegation • Execution • Recovery  │
├─────────────────────────────────────────────────────────┤
│                       AGENT CORE                         │
│                                                         │
│ LLM • Context • State • Policies • Decision Loop        │
├─────────────────────────────────────────────────────────┤
│                       CAPABILITIES                       │
│                                                         │
│ Tools • Search • RAG • Code • APIs • Databases          │
├─────────────────────────────────────────────────────────┤
│                         MEMORY                          │
│                                                         │
│ History • Working Memory • Long-Term Memory • State     │
├─────────────────────────────────────────────────────────┤
│                EVALUATION & OBSERVABILITY               │
│                                                         │
│ Traces • Metrics • Tests • Cost • Latency • Failures    │
├─────────────────────────────────────────────────────────┤
│                    SAFETY & SECURITY                    │
│                                                         │
│ Permissions • Validation • Sandboxing • Guardrails      │
└─────────────────────────────────────────────────────────┘
```

The goal is to understand each layer independently before composing them.

---

## ⚡ The First Mental Model

Before reaching for a framework, start with the smallest possible agent loop:

```python
while not task_complete:

    decision = model(
        goal=goal,
        context=context,
        available_tools=tools
    )

    if decision.requires_tool:
        result = execute_tool(decision.tool)
        context.append(result)

    else:
        return decision.final_answer
```

Real agents get significantly more complicated — invalid tool calls, context limits, hallucinated actions, auth failures, cost constraints, safety checks. That complexity is what the experiments here are for.

---

## 🧪 How experiments work

Each experiment tries to answer a specific question:

- Can an LLM reliably choose between multiple tools?
- What happens when a tool fails mid-task?
- How much context should an agent actually retain?
- When does RAG help, and when does it add noise?
- When should an agent delegate to another agent?

Every experiment documents the same things:

```text
Problem → Hypothesis → Architecture → Implementation
→ Observed Behavior → Failure Cases → Lessons → Next Iteration
```

Not every implementation will be clean. Some will be wrong. That's part of the record.

---

## 📂 Repository Structure

```text
Building-AI-agents/
│
├── AI/
│   ├── agents/
│   ├── memory/
│   ├── tools/
│   ├── prompts/
│   ├── rag/
│   ├── orchestration/
│   └── evaluation/
│
├── python/
│   └── indoor/
│       ├── server/
│       └── utils/
│
├── experiments/
│   ├── single-agent/
│   ├── tool-use/
│   ├── memory/
│   ├── rag/
│   ├── planning/
│   ├── multi-agent/
│   └── evaluation/
│
├── docs/
│   ├── architecture/
│   ├── concepts/
│   └── experiments/
│
├── examples/
├── tests/
├── scripts/
└── README.md
```

Structure will grow as the engineering scope does.

---

## 🚀 Quick Start

```bash
git clone https://github.com/ChandanLabs/Building-AI-agents.git
cd Building-AI-agents
```

Create and activate a Python environment:

```bash
cd python/indoor
python -m venv .venv
```

**Linux / macOS**
```bash
source .venv/bin/activate
```

**Windows**
```powershell
.venv\Scripts\activate
```

Install dependencies and configure environment:

```bash
pip install -r requirements.txt
cp .env.example .env
# Add your API keys to .env — never commit them
```

---

## 📚 Learning Path

The repo follows a progression rather than jumping into frameworks first:

```text
01. LLM Fundamentals      →  02. Structured Outputs   →  03. Tool Calling
04. Agent Loops           →  05. Context & State       →  06. RAG
07. Memory                →  08. Planning              →  09. Reflection / Recovery
10. Multi-Agent Systems   →  11. Evaluation            →  12. Security & Safety
                                                        →  13. Production Architecture
```

Understand the primitive before relying on the framework.

---

## 📊 Engineering questions I care about

| Area | Question |
|---|---|
| **Correctness** | Did the agent actually achieve the goal? |
| **Reliability** | Does it behave consistently across runs? |
| **Observability** | Can we see what happened and why? |
| **Latency** | How long does execution take? |
| **Cost** | How many model and tool calls did it take? |
| **Safety** | What actions is the agent allowed to perform? |
| **Recoverability** | What happens when something goes wrong? |
| **Scalability** | Does the architecture hold at larger workloads? |
| **Maintainability** | Can another developer understand and extend it? |

The goal isn't a demo that produces a cool answer. It's a system you can explain, measure, and improve.

---

## 🔬 Evaluation

Agent systems can look impressive while being unreliable. Evaluation is part of the build, not an afterthought.

Experiments will track: task success rate, tool selection accuracy, completion rate, retry count, failure rate, token usage, latency, cost, and retrieval quality.

The full execution trajectory matters — not only the final response.

---

## 🔐 Safety

More capabilities mean more consequences when things go wrong. Every experiment considers tool permissions, input and output validation, secrets management, sandboxing, prompt injection risks, and when a human should be in the loop.

The goal is useful autonomy inside explicit limits — not maximum autonomy.

---

## 🗺️ Roadmap

### Phase 1 — Foundations
- [x] Repository setup
- [ ] LLM API fundamentals
- [ ] Structured outputs
- [ ] Function calling
- [ ] Basic tool-use agent
- [ ] Agent execution loop

### Phase 2 — Context & Knowledge
- [ ] Conversation state management
- [ ] Context compression
- [ ] Embeddings
- [ ] Vector retrieval
- [ ] RAG agent
- [ ] Memory experiments

### Phase 3 — Reasoning & Planning
- [ ] ReAct-style execution
- [ ] Task decomposition
- [ ] Planning
- [ ] Reflection
- [ ] Retry and recovery strategies

### Phase 4 — Multi-Agent Systems
- [ ] Specialist agents
- [ ] Delegation
- [ ] Supervisor architecture
- [ ] Agent handoffs
- [ ] Parallel execution
- [ ] Shared vs isolated state

### Phase 5 — Evaluation
- [ ] Agent trajectories
- [ ] Automated evaluation
- [ ] Regression tests
- [ ] Tool-use evaluation
- [ ] Cost and latency tracking

### Phase 6 — Production Engineering
- [ ] API layer
- [ ] Authentication
- [ ] Observability
- [ ] Secure tool execution
- [ ] Sandboxing
- [ ] Deployment

---

## 👀 Who this is for

**Developers** — if you want to understand how agents actually work, trace failure modes, study tool calling and memory, or reproduce an experiment and see what breaks.

**Recruiters** — this repo is a public engineering record, not a finished product. What matters here is the progression: what was tried, what failed, what got measured, and what changed because of it. The code is less interesting than the engineering decisions behind it.

---

## 🤝 Contributing

Thoughtful contributions are welcome. A good one includes a clear problem statement, the reason for the approach, an implementation, and some evidence of whether it worked.

---

<div align="center">

## 🧠 Build systems. Measure behavior. Learn from failure.

**Building AI Agents — one experiment at a time.**

⭐ Star the repo if you want to follow the build.

</div>
