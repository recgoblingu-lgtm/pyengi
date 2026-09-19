# AI Agents

[![Status](https://img.shields.io/badge/status-in%20development-orange)](#project-status)
[![Version](https://img.shields.io/badge/version-0.1.0-blue)](#project-status)
[![Tests](https://img.shields.io/badge/tests-WIP-yellow)](#testing)
[![Coverage](https://img.shields.io/badge/coverage-not%20measured-lightgrey)](#testing)
[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Agents](https://img.shields.io/badge/agents-multi--agent-blueviolet)](#agent-types)
[![Tools](https://img.shields.io/badge/tool%20use-configurable-purple)](#tools)
[![Memory](https://img.shields.io/badge/memory-extensible-8A2BE2)](#memory)
[![Planning](https://img.shields.io/badge/planning-supported-1f6feb)](#planning-and-execution)
[![Human Review](https://img.shields.io/badge/human%20review-supported-red)](#safety-and-permissions)
[![Contributions](https://img.shields.io/badge/contributions-welcome-brightgreen)](CONTRIBUTING.md)
[![Documentation](https://img.shields.io/badge/docs-in%20progress-blue)](#documentation)

A flexible framework for building, testing, and running AI agents that can plan tasks, use tools, remember relevant context, and collaborate while keeping people in control.

> **Project status:** This project is in active development. APIs, agent behavior, configuration formats, and supported model providers may change.

## What are AI agents?

An AI agent is a software component that receives a goal, decides what steps may be needed, optionally uses approved tools, and returns a result. Unlike a simple one-shot prompt, an agent can maintain state across a task, inspect intermediate results, and choose between several permitted actions.

This project treats agents as constrained software components rather than unrestricted autonomous systems. Every agent should have a defined purpose, limited permissions, observable behavior, and clear stopping conditions.

## Features

- **Goal-based execution** for turning a natural-language task into a sequence of actions.
- **Specialized agents** for planning, research, coding, reviewing, coordination, and more.
- **Tool use** for approved functions, APIs, files, databases, and services.
- **Multi-agent workflows** that allow agents to delegate work and share structured results.
- **Planning and retries** for handling intermediate failures without losing the task context.
- **Short-term memory** for conversation and task state.
- **Extensible long-term memory** for project knowledge and reusable context.
- **Structured output** for logs, tool calls, events, results, and evaluation data.
- **Human approval checkpoints** for sensitive or irreversible actions.
- **Configurable limits** for steps, time, tokens, tools, and cost.
- **Evaluation support** for measuring quality, reliability, and regression behavior.

## Agent types

The framework can support several kinds of agents. A project does not need to use every type.

| Agent | Responsibility | Typical tools |
| --- | --- | --- |
| **Planner** | Breaks a large goal into smaller, ordered tasks. | Task lists, dependency graphs |
| **Researcher** | Collects, compares, and summarizes information from approved sources. | Search, fetch, document readers |
| **Coder** | Writes, updates, and explains code inside an allowed workspace. | File access, tests, linters |
| **Debugger** | Investigates failures and proposes or applies fixes. | Logs, test runners, repository tools |
| **Reviewer** | Checks code, documents, or results against defined requirements. | Diff viewers, validators, test reports |
| **Data analyst** | Inspects data and produces calculations or visualizations. | Data files, notebooks, analysis tools |
| **Coordinator** | Assigns work to other agents and combines their results. | Agent registry, task queue |
| **Assistant** | Handles interactive questions and lightweight developer workflows. | Documentation, project metadata |

## How an agent works

A typical agent run follows this loop:

```text
Goal
  ↓
Understand the task
  ↓
Create or update a plan
  ↓
Choose an approved action or tool
  ↓
Validate the result
  ↓
Continue, ask for review, or stop
  ↓
Return a structured result
```

An agent should stop when the task is complete, when it reaches a configured limit, when it lacks the required permission, or when human input is needed.

## Planning and execution

Agents can use a plan to make complex work easier to inspect. A plan may include dependencies, expected outputs, retry rules, and approval requirements.

```yaml
name: project-planner
role: Break development goals into safe, testable tasks.
max_steps: 10
max_retries: 2
requires_human_approval: false
tools:
  - project-inspector
  - task-list
  - documentation-reader
```

Planning does not guarantee a correct result. Each important step should still be validated, especially when an agent can change files, call external services, or affect user data.

## Tools

Tools give agents controlled access to actions that a language model cannot perform by itself. A tool should have a clear name, documented input schema, predictable output, and an explicit permission boundary.

Example tool categories include:

- **Workspace tools** for reading files, writing approved files, and inspecting project structure.
- **Development tools** for running tests, formatters, linters, and build commands.
- **Research tools** for searching approved sources and reading documents.
- **Data tools** for querying, transforming, and visualizing structured data.
- **Communication tools** for preparing messages or drafts that require review before sending.
- **Service tools** for interacting with APIs through configured credentials and limited scopes.

Agents should never receive every available tool by default. Use the smallest tool set that can complete the task.

## Memory

Memory allows an agent to use relevant information from earlier steps or previous runs. The framework may support several memory layers:

| Memory layer | Lifetime | Example |
| --- | --- | --- |
| Working memory | One reasoning step or tool call | The current tool result |
| Task memory | One task run | The active plan and completed steps |
| Conversation memory | One conversation | User preferences and prior questions |
| Project memory | Multiple runs | Repository conventions and documented decisions |
| Long-term memory | Persistent | Approved knowledge stored for future tasks |

Memory should be scoped, reviewable, and protected from unnecessary sensitive data. Stored context must not automatically grant an agent new permissions.

## Multi-agent workflows

A multi-agent workflow divides work between specialized agents. For example:

```text
Coordinator
├── Planner      → creates the work plan
├── Researcher   → gathers relevant information
├── Coder       → implements the change
└── Reviewer     → checks the result
```

A coordinator should pass structured tasks to other agents and preserve the source of each result. Agents should not silently override one another when their outputs disagree; conflicts should be surfaced for review.

## Installation

Clone the repository and create a virtual environment:

```bash
git clone https://github.com/<your-username>/<your-repository>.git
cd <your-repository>
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\\Scripts\\activate
python -m pip install --upgrade pip
python -m pip install -e ".[dev]"
```

Copy the example configuration before running the project:

```bash
cp .env.example .env
```

Keep API keys and other secrets in environment variables or a local secret manager. Never commit `.env` files containing real credentials.

## Quick start

Run the command-line interface:

```bash
ai-agents --help
ai-agents agents list
ai-agents run --agent planner --task "Plan a small Python utility"
```

A basic Python integration may look like this:

```python
from ai_agents import Agent, Task

planner = Agent(
    name="planner",
    role="Break goals into actionable steps",
    tools=[],
)

result = planner.run(Task("Plan a small Python utility"))
print(result.summary)
```

The exact API is subject to change while the project is in development.

## Configuration

Configure each agent with a narrow role, explicit tools, model settings, and clear limits.

```yaml
name: planner
model: your-model-name
system_prompt: >-
  Break the user goal into practical steps. Do not execute external actions.
tools: []
max_steps: 8
max_retries: 2
requires_human_approval: false
output_format: markdown
```

Recommended configuration principles:

- Grant each agent only the tools it needs.
- Set time, step, token, and cost limits.
- Require approval before external, destructive, financial, legal, or account-level actions.
- Log tool calls and important decisions without storing unnecessary sensitive data.
- Treat model output as untrusted input until it has been validated.
- Make failure and timeout behavior explicit.

## Safety and permissions

AI agents can make incorrect assumptions, misuse tools, or produce unsafe output. This project is designed around constrained permissions, observable execution, and human oversight.

Do not use an agent to make high-impact decisions without appropriate review. Validate generated code, restrict filesystem and network access, protect personal information, and test failure cases before enabling automation.

Agents should be reviewed before they are allowed to:

- Delete or overwrite important data.
- Send messages or publish content externally.
- Change account access, security, ownership, or billing.
- Submit legal, financial, medical, employment, or government records.
- Run commands with broad system or production permissions.

## Observability

A useful agent run should make it possible to answer:

- What goal did the agent receive?
- Which model and configuration were used?
- Which tools were called, with what inputs?
- What files or services were changed?
- Where did the agent stop, fail, retry, or request approval?
- What final result was returned?

Logs should redact secrets and should retain only the personal or sensitive information needed for debugging and evaluation.

## Evaluation

Agent quality should be measured with repeatable tasks rather than anecdotal examples alone. Useful evaluation dimensions include:

- **Task success** — whether the requested outcome was achieved.
- **Correctness** — whether the result is accurate and valid.
- **Reliability** — whether behavior remains consistent across runs.
- **Safety** — whether the agent respects permissions and approval boundaries.
- **Efficiency** — how many steps, tool calls, tokens, and retries were needed.
- **Recoverability** — whether the agent handles errors and partial results safely.

## Testing

Run the test suite and quality checks with:

```bash
pytest
ruff check .
ruff format --check .
```

Agent tests should cover successful workflows and failure behavior, including invalid tool inputs, unavailable services, timeouts, prompt injection attempts, permission errors, malformed model output, and partial results.

## Project status and roadmap

The project is currently experimental. The following milestones are planned:

- [ ] Define stable agent, tool, memory, and event interfaces.
- [ ] Add a local single-agent runner.
- [ ] Add structured tool-call and execution logs.
- [ ] Add configurable model providers.
- [ ] Add multi-agent orchestration.
- [ ] Add memory adapters with clear retention controls.
- [ ] Add evaluation datasets and regression tests.
- [ ] Add configurable approval checkpoints.
- [ ] Publish versioned documentation and examples.

## Documentation

Documentation is in progress. Planned documentation includes:

- Agent creation guide
- Tool development guide
- Configuration reference
- Multi-agent workflow examples
- Safety and permission model
- Evaluation and testing guide
- API reference

## Contributing

Contributions are welcome. For larger changes, open an issue before implementation so the design can be discussed. Keep pull requests focused, add tests for changed behavior, document new configuration, and run the formatter and test suite before submitting.

Please do not include API keys, private data, production logs, or unreviewed agent traces in issues or pull requests.

## License

This project is intended to be released under the [MIT License](LICENSE). Add the complete license text to `LICENSE` before publishing.

## Support

Use GitHub Issues for bug reports and feature requests. Include the project version, environment, configuration summary, reproduction steps, and relevant logs with secrets removed.

<!--
Badge customization:
- Replace <your-username>/<your-repository> with the real GitHub repository.
- Replace the static status and test badges with live GitHub Actions badges when CI is configured.
- Add provider-specific badges only when those providers are actually supported.
-->

