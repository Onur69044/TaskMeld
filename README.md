<p align="center">
  <strong>English</strong>
  &nbsp;·&nbsp;
  <a href="./README.zh-CN.md">简体中文</a>
  &nbsp;·&nbsp;
  <a href="https://taskmeld.com">Website</a>
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/taskmeld"><img src="https://img.shields.io/npm/v/taskmeld.svg?style=flat-square&color=cb3837&labelColor=161b22&logo=npm&logoColor=white" alt="npm version"/></a>
  <a href="./package.json"><img src="https://img.shields.io/node/v/taskmeld.svg?style=flat-square&color=5fa04e&labelColor=161b22&logo=nodedotjs&logoColor=white" alt="node"/></a>
</p>

<br/>

<h1 align="center">TaskMeld</h1>
<h3 align="center">Agent Pipeline Orchestration Platform</h3>
<p align="center">Compose <strong>OpenClaw agents</strong> into executable pipelines — define, run, observe, and iterate with <strong>Wevra Agent</strong>.</p>

<br/>

> [!TIP]
> **The Stack:**
> - **OpenClaw** — Agent execution runtime (pipeline nodes)
> - **TaskMeld** — Pipeline orchestration engine (DAG, scheduling, artifacts)
> - **Wevra** — Built-in Agent (operates pipelines via natural language)

<br/>

## 🏗️ Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                     User / Wevra Agent                    │
│         (Natural language pipeline management)               │
└───────────────────────────┬─────────────────────────────────┘
                            │
┌───────────────────────────▼─────────────────────────────────┐
│                    TaskMeld (This Repo)                      │
│  ┌──────────────────────────────────────────────────────┐   │
│  │ Pipeline Engine (DAG · Scheduler · State Machine)    │   │
│  │  • Node dependency graphs                            │   │
│  │  • Parallel groups & routing branches                │   │
│  │  • Per-node retry & artifact tracking                │   │
│  └──────────────────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────────────────┐   │
│  │ Wevra Agent (40 tools · ReAct loop · Memory)      │   │
│  │  • Pipeline CRUD & monitoring                        │   │
│  │  • Agent lifecycle management                        │   │
│  │  • Failure diagnosis & optimization                  │   │
│  └──────────────────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────────────────┐   │
│  │ OpenClaw Management (Detect · Install · Gateway)    │   │
│  │  • Auto-detect local OpenClaw installation          │   │
│  │  • One-click install (npm install -g openclaw)      │   │
│  │  • Gateway start/stop/status                        │   │
│  └──────────────────────────────────────────────────────┘   │
└───────────────────────────┬─────────────────────────────────┘
                            │ Gateway RPC
┌───────────────────────────▼─────────────────────────────────┐
│                    OpenClaw Gateway                          │
│  (Agent registry · Session management · Event relay)         │
└───────────────────────────┬─────────────────────────────────┘
                            │
┌───────────────────────────▼─────────────────────────────────┐
│                  OpenClaw Agent Runtime                      │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐      │
│  │  Node 1  │ │  Node 2  │ │  Node 3  │ │  Node 4  │      │
│  │ (Agent A)│ │ (Agent B)│ │ (Agent C)│ │ (Agent D)│      │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘      │
│  (Each pipeline node binds an OpenClaw agent)                │
└─────────────────────────────────────────────────────────────┘
```

**How it works:**
1. **TaskMeld** orchestrates pipelines as DAGs with dependencies, routing, and retries
2. Each pipeline node binds an **OpenClaw agent** that executes the actual work
3. **Wevra Agent** operates the entire stack through natural language
4. **OpenClaw Management** module handles auto-detection, one-click install, and Gateway lifecycle

<br/>

## 🚀 Why TaskMeld?

### The Problem
Managing complex agent pipelines requires constant manual intervention: creating configurations, monitoring runs, diagnosing failures, and iterating on designs. This is time-consuming, error-prone, and doesn't scale.

### The Solution: Three-Layer Stack

| Layer | Component | Role |
|-------|-----------|------|
| **Execution** | OpenClaw | Agent runtime — each node runs an OpenClaw agent |
| **Orchestration** | TaskMeld | Pipeline engine — DAG, scheduling, state management |
| **Intelligence** | Wevra | Agent — operates pipelines via natural language |

### Why Not Just OpenClaw?

OpenClaw supports direct agent-to-agent communication (via OpenClaw config files), so why add TaskMeld?

**OpenClaw alone:**
- Point-to-point calls — agents communicate directly, no orchestration
- Config-based wiring — agent-to-agent calls require manual OpenClaw config files, inflexible
- Manual coordination — you define the flow in code each time
- No retry or recovery — if something fails, you start over

**With TaskMeld:**
- **DAG orchestration** — Define complex dependencies, parallel groups, and routing branches declaratively
- **State persistence** — Every execution is recorded; retry failed nodes, resume interrupted runs
- **Artifact tracking** — Complete lineage of data flowing between agents
- **Scheduled execution** — Cron-based or event-driven triggers
- **Wevra Agent** — Manage the entire stack through natural language

**In short:** OpenClaw executes agents; TaskMeld orchestrates them.

**Result:** Transform from *you operate the pipelines* to *Wevra operates the pipelines, and you direct*.

<br/>

## ✨ Key Features

### 🔧 Pipeline Engine (Core)

- **DAG Orchestration** — Node dependency graphs, parallel groups, routing branches
- **Per-Node Retry** — Automatic retry with configurable policies
- **State Persistence** — All state stored as JSON files, zero external database
- **Artifact Tracking** — Complete lineage of pipeline outputs
- **OpenClaw Integration** — Seamless agent execution via Gateway

### 🤖 Wevra Agent

- **40 Built-in Tools** — Pipeline CRUD, Agent management via Gateway, System monitoring, Memory, Skills
- **Natural Language Interface** — "List all pipelines", "Run the data processing pipeline", "What failed in the last run?"
- **Multi-Provider LLM Support** — DeepSeek, OpenAI, and custom providers
- **ReAct Loop** — Standard Reasoning + Acting pattern for intelligent task execution
- **Real-time Streaming** — WebSocket-based streaming of thinking process and execution results
- **Permission Control** — Three modes: Plan (read-only), Normal (confirm writes), Auto (full access)
- **Cross-Session Memory** — Remembers preferences, patterns, and solutions

### 🔌 OpenClaw Integration

- **Agent Registry** — List, create, update, delete agents via Gateway
- **Session Management** — Send messages, track conversations, view history
- **Event Relay** — Real-time events from agent execution
- **Delegated Execution** — Pipeline nodes delegate work to OpenClaw agents
- **Auto-Detection & Installation** — `init` command auto-detects local OpenClaw, one-click install, and generates Gateway config

### 🖥️ Multi-Interface Access

- **Web Console** — React 19 dashboard with DAG visualization, WevraChatPanel, and monitoring
- **CLI Tool** — Full lifecycle management for automation and scripting
- **WebSocket API** — 19 methods for real-time control and observability

<br/>

## 🎯 Use Cases

### 1. Intelligent Pipeline Management
```
You: Create a pipeline that processes daily sales data using OpenClaw agents
Wevra: [Creates pipeline with 4 nodes, each binding an OpenClaw agent]
       Pipeline created. Each node will execute via OpenClaw agents.
       Would you like me to schedule it to run daily at 9 AM?
```

### 2. Agent Lifecycle Management
```
You: List all OpenClaw agents and their status
Wevra: [Calls agent_list via Gateway]
       You have 5 agents: DataCollector, Analyzer, Reporter, Cleaner, Notifier
       
You: Create a new OpenClaw agent for customer segmentation
Wevra: [Creates agent via Gateway]
       Agent "Segmenter" created and ready for pipeline assignment.
```

### 3. Autonomous Failure Recovery
```
You: The sales pipeline failed last night. What happened?
Wevra: [Analyzes pipeline status and OpenClaw agent logs]
       Root cause: OpenClaw agent "DataCollector" timed out on Node 2.
       Impact: 3 downstream nodes skipped.
       Action: I've increased the timeout and added a fallback agent.
```

### 4. Cross-System Orchestration
```
You: Create a pipeline that chains 3 OpenClaw agents: collect → analyze → report
Wevra: [Creates DAG with dependencies]
       Pipeline configured. Agent execution order:
       1. DataCollector (OpenClaw) → artifacts
       2. Analyzer (OpenClaw) → consumes artifacts → produces analysis
       3. Reporter (OpenClaw) → consumes analysis → generates report
```

<br/>

## 📦 Prerequisites

| Category | Requirement | Notes |
|----------|-------------|-------|
| **Required** | Node ≥ 18 | Runtime environment |
| **Optional** | OpenClaw ≥ 5.20 | Agent execution runtime — can be auto-installed via `taskmeld init` |
| **Platform** | Windows | macOS and Linux not yet fully tested |

> [!IMPORTANT]
> Pipeline execution requires OpenClaw. TaskMeld's `init` command can auto-detect a local installation or install OpenClaw with one command and generate Gateway configuration. You can also install OpenClaw manually and connect via `init`, or configure a remote Gateway address.

<br/>

## 🔧 Install

### 1. Install TaskMeld

```bash
npm install -g taskmeld
```

### 2. Initialize (First-time Setup)

```bash
taskmeld init
```

The `init` command guides you through:
- Language selection (English / Chinese)
- Auto-detection of local OpenClaw installation
- **OpenClaw found** → auto-connect to running Gateway
- **OpenClaw not found** → three options:
  - **Auto-install** — install OpenClaw via npm and generate config
  - **Remote Gateway** — configure remote OpenClaw Gateway URL and Token
  - **Manual install** — install OpenClaw yourself and re-run `init`
- Optional model configuration (after auto-install)

Configuration is saved to `~/.taskmeld/config.json`.

#### Non-interactive Mode

```bash
# Auto-install OpenClaw and configure
taskmeld init --auto-install

# Configure remote Gateway
taskmeld init --remote ws://your-gateway:18789 --token your-token

# Direct URL and Token (for CI/CD)
taskmeld init --url ws://127.0.0.1:18789 --token your-token
```

<br/>

## 🚀 Quick Start

### Standard Flow

```bash
# Initialize — auto-detect OpenClaw, install if missing, configure Gateway
taskmeld init

# Start the backend daemon (auto-starts local OpenClaw Gateway)
taskmeld server start

# Open browser to Web Console
# Visit http://127.0.0.1:54320 and use the Wevra chat panel to manage pipelines
```

### Unattended Flow (CI/CD, automation)

```bash
# One-step: install + configure + start
taskmeld init --auto-install && taskmeld server start
```

### Command Reference

#### Init

| Command | Description |
|---------|-------------|
| `taskmeld init` | Interactive — auto-detect/install OpenClaw, configure Gateway |
| `taskmeld init --auto-install` | Non-interactive — auto-install OpenClaw and configure |
| `taskmeld init --remote <url> --token <token>` | Non-interactive — configure remote Gateway |
| `taskmeld init --url <url> --token <token>` | Non-interactive — specify Gateway URL and Token directly |
| `taskmeld init --skip-model-config` | Skip model configuration (after auto-install) |

#### Server

| Command | Description |
|---------|-------------|
| `taskmeld server start` | Start backend daemon (auto-starts local OpenClaw Gateway) |
| `taskmeld server status` | Check server status |
| `taskmeld server stop` | Stop backend daemon |

#### Pipeline & Management

| Command | Description |
|---------|-------------|
| `taskmeld pipeline list` | List available pipelines |
| `taskmeld pipeline start <id>` | Start a pipeline run (executes OpenClaw agents) |
| `taskmeld pipeline watch <id>` | Monitor a run in real-time via WebSocket |
| `taskmeld pipeline status <id>` | Get current pipeline status |
| `taskmeld pipeline stop <id>` | Stop a running pipeline |
| `taskmeld pipeline retry-node <id> <node>` | Retry a failed node |
| `taskmeld agent list` | List registered OpenClaw agents |
| `taskmeld artifact list` | Browse pipeline artifacts |

Full command reference: `taskmeld --help` or [CLI docs](docs/cli.md).

<br/>

## 💬 Chat with Wevra

After starting the server, access the Web Console at `http://127.0.0.1:54320` and use the Wevra chat panel:

```
You: What OpenClaw agents do I have?
Wevra: You have 5 agents registered: DataCollector, Analyzer, Reporter, Cleaner, Notifier

You: Create a pipeline using DataCollector and Analyzer
Wevra: [Creates pipeline with 2 nodes binding these OpenClaw agents]
       Pipeline created. Node 1 uses DataCollector, Node 2 uses Analyzer.

You: Run the pipeline and monitor execution
Wevra: Pipeline started. Monitoring OpenClaw agent execution...
       Node 1 (DataCollector): ✅ Complete
       Node 2 (Analyzer): ✅ Complete
       All agents completed successfully.
```

<br/>

## 🏗️ Directory Structure

| Directory | Purpose |
|-----------|---------|
| `src/openclaw/` | **OpenClaw Management** — Auto-detect, install, Gateway lifecycle |
| `src/wevra/` | **Wevra Agent** — Brain, Loop, Tools, Memory, Skills |
| `src/pipeline/` | Pipeline engine (DAG, scheduler, execution) |
| `src/transport/` | WebSocket transport (19 methods) |
| `src/services/` | Service layer (PipelineService, AgentService, SessionService) |
| `src/gateway/` | **OpenClaw Gateway** integration |
| `src/init/` | Init orchestrator — detection, installation, configuration flow |
| `web/` | React frontend with WevraChatPanel |

<br/>

## 🛠️ Development

```bash
npm install          # Install dependencies
npm run build        # Build
npm run typecheck    # Type check only
npm run lint         # Lint
npm run format       # Format code
npm run format:check # Check code format
npm test             # Run tests
npm run dev          # Start backend dev server (tsx hot-reload)
npm run dev:cli      # Start CLI dev mode
npm run dev:web      # Start frontend dev server (Vite HMR)
```

| Layer | Technology |
|-------|------------|
| Language | TypeScript (strict, CommonJS) |
| Runtime | Node.js |
| Agent | Wevra (custom, zero dependencies) |
| Agent Execution | **OpenClaw** |
| Backend HTTP | Node.js built-in `http` |
| WebSocket | `ws` |
| Frontend | React 19 + Vite 7 |
| CSS | Tailwind CSS 4 |
| Testing | Vitest |

<br/>

## 🌟 Roadmap

### Current
- ✅ DAG pipeline engine with OpenClaw agent execution
- ✅ Wevra Agent with 40 tools
- ✅ Agent lifecycle management via OpenClaw Gateway
- ✅ OpenClaw auto-detection, installation, and Gateway management
- ✅ CLI, WebSocket API, and Web console
- ✅ Server bound to localhost (security hardening)

### Upcoming
- 🔶 Pipeline-scoped conversations
- 🔶 Cross-pipeline access control
- 🔶 Enhanced memory system

### Future
- 📋 Multi-agent collaboration patterns
- 📋 Advanced scheduling (cron, event-driven)
- 📋 Plugin ecosystem

<br/>

## 🤝 Contributing

We welcome contributions! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

**Areas where help is needed:**
- Testing on macOS and Linux
- Additional LLM provider integrations
- OpenClaw agent tool implementations
- Documentation improvements

<br/>

## 📄 License

MIT — see [LICENSE](LICENSE)

<br/>

---

<p align="center">
  <strong>OpenClaw + TaskMeld + Wevra = Automated Agent Pipelines</strong><br/>
  <sub>Execute · Orchestrate · Automate</sub>
</p>
