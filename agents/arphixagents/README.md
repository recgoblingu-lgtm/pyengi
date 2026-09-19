<div align="center">
  <img src="assets/arphix-agents-logo.png" alt="Arphix Agents logo" width="180" />

  # Arphix Agents

  **A modular platform for building reliable, tool-using AI agents.**

  [![Status](https://img.shields.io/badge/status-in%20development-orange)](#project-status)
  [![Version](https://img.shields.io/badge/version-0.1.0-1677ff)](#project-status)
  [![Tests](https://img.shields.io/badge/tests-WIP-yellow)](#testing)
  [![Coverage](https://img.shields.io/badge/coverage-not%20measured-lightgrey)](#testing)
  [![Agents](https://img.shields.io/badge/agents-multi--agent-7c3aed)](#agent-types)
  [![Tools](https://img.shields.io/badge/tools-configurable-06b6d4)](#tools)
  [![Memory](https://img.shields.io/badge/memory-extensible-0891b2)](#memory)
  [![Planning](https://img.shields.io/badge/planning-supported-2563eb)](#planning)
  [![Human Review](https://img.shields.io/badge/human%20review-supported-dc2626)](#safety-and-permissions)
  [![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)](https://www.python.org/)
  [![License](https://img.shields.io/badge/license-MIT-22c55e)](LICENSE)
  [![Contributions](https://img.shields.io/badge/contributions-welcome-brightgreen)](CONTRIBUTING.md)
</div>

## About Arphix Agents

Arphix Agents is a framework for creating AI agents that can understand goals, plan work, use approved tools, maintain relevant context, and collaborate with other agents. It is designed for developer workflows, research, automation, data tasks, and other projects that benefit from structured agent behavior.

Arphix treats agents as **constrained software components**, not unrestricted autonomous systems. Each agent should have a clear role, limited permissions, observable actions, and a defined stopping condition.

> **Project status:** Arphix Agents is experimental and actively changing. APIs, configuration formats, agent behavior, and supported model providers may change before the first stable release.

## Highlights

- **Specialized agents** for planning, research, coding, debugging, review, and coordination.
- **Tool calling** for approved functions, files, APIs, databases, and development commands.
- **Multi-agent workflows** for delegating work and combining structured results.
- **Planning and retries** for complex tasks and recoverable failures.
- **Short-term and long-term memory** with configurable scope and retention.
- **Human approval checkpoints** for sensitive or irreversible operations.
- **Execution limits** for steps, time, tokens, tools, retries, and cost.
- **Structured events and logs** for debugging, evaluation, and observability.
- **Provider flexibility** so applications can select the model integrations they need.

## Agent types

| Agent | Purpose | Example work |
| --- | --- | --- |
| **Planner** | Breaks a broad goal into ordered tasks. | Create an implementation plan. |
| **Researcher** | Gathers and compares information from approved sources. | Summarize technical options. |
| **Coder** | Writes or updates code inside an allowed workspace. | Implement a feature and add tests. |
| **Debugger** | Investigates failures and proposes safe fixes. | Analyze a failing test. |
| **Reviewer** | Checks results against requirements. | Review a pull request or report. |
| **Data agent** | Inspects, transforms, and explains structured data. | Produce a data summary. |
| **Coordinator** | Routes work between specialized agents. | Run a research-to-code workflow. |
| **Assistant** | Handles interactive developer tasks. | Answer project questions. |

## How it works

```text
User goal
   ↓
Agent understands the request
   ↓
Agent creates or updates a plan
   ↓
Agent selects an approved tool
   ↓
Tool result is validated
   ↓
Agent continues, requests review, or stops
   ↓
Structured result is returned
```

An agent should stop when the task is complete, when it reaches a configured limit, when it lacks permission, or when a human decision is required.

## Planning

Planning makes complex work easier to inspect and evaluate. A plan can contain dependencies, expected outputs, retry behavior, and approval requirements.

```yaml
name: arphix-planner
role: Break development goals into safe, testable tasks.
max_steps: 10
max_retries: 2
requires_human_approval: false
tools:
  - project-inspector
  - task-list
  - documentation-reader
```

Planning is not a guarantee of correctness. Important actions and outputs must still be validated.

## Tools

Tools give agents controlled access to actions that a language model cannot perform by itself. Every tool should have a documented input schema, predictable output, explicit permissions, and a useful error response.

Common tool categories include:

- **Workspace tools** for reading files and writing approved files.
- **Development tools** for running tests, formatters, linters, and builds.
- **Research tools** for searching approved sources and reading documents.
- **Data tools** for querying, transforming, and visualizing datasets.
- **Communication tools** for preparing drafts that require review before sending.
- **Service tools** for interacting with APIs through limited credentials and scopes.

Agents should receive the smallest tool set that can complete their assigned task. Do not expose every available tool by default.

## Memory

Arphix Agents can use scoped memory to preserve useful context across a workflow. Memory should be reviewable and must not silently grant additional permissions.

| Memory layer | Lifetime | Example |
| --- | --- | --- |
| Working memory | One step or tool call | The current tool result |
| Task memory | One task run | The active plan and completed steps |
| Conversation memory | One conversation | Relevant user preferences |
| Project memory | Multiple runs | Repository conventions |
| Long-term memory | Persistent | Approved reusable knowledge |

Avoid storing secrets or unnecessary personal information. Define retention and deletion behavior before enabling persistent memory.

## Multi-agent workflows

A multi-agent workflow divides work among specialists:

```text
Arphix Coordinator
├── Planner      → creates the task plan
├── Researcher   → gathers relevant information
├── Coder        → implements the change
└── Reviewer     → checks the result
```

Agents should exchange structured results and preserve the source of important claims. If agents disagree, the conflict should be surfaced for review instead of being silently hidden.

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

Copy the example environment file if the project provides one:

```bash
cp .env.example .env
```

Keep API keys and other secrets in environment variables or a secret manager. Never commit real credentials.

## Quick start

The planned command-line interface may look like this:

```bash
arphix --help
arphix agents list
arphix run --agent planner --task "Plan a small Python utility"
```

A conceptual Python integration may look like this:

```python
from arphix_agents import Agent, Task

planner = Agent(
    name="planner",
    role="Break goals into actionable steps",
    tools=[],
)

result = planner.run(Task("Plan a small Python utility"))
print(result.summary)
```

The public API is not final. Use the project source and release documentation as the authoritative reference once implementation begins.

## Safety and permissions

AI agents can make incorrect assumptions, misuse tools, or produce unsafe output. Arphix Agents is designed around least-privilege tools, observable execution, configurable limits, and human oversight.

Require review before an agent is allowed to:

- Delete or overwrite important data.
- Send messages or publish content externally.
- Change account access, security, ownership, or billing.
- Submit legal, financial, medical, employment, or government records.
- Run commands with broad system or production permissions.

Treat model output as untrusted until it has been validated. Restrict filesystem and network access, redact secrets from logs, and test failure behavior before enabling automation.

## Observability

A useful run should make it possible to identify the goal, model, configuration, tools, inputs, outputs, retries, failures, approvals, and final result. Logs should contain enough information for debugging without retaining unnecessary sensitive data.

## Evaluation

Evaluate agents with repeatable tasks rather than demonstrations alone. Useful metrics include task success, correctness, reliability, safety, efficiency, recoverability, and adherence to tool permissions.

## Testing

Run the test suite and quality checks with:

```bash
pytest
ruff check .
ruff format --check .
```

Test successful workflows and failure behavior, including invalid tool inputs, timeouts, unavailable services, permission errors, prompt injection attempts, malformed model output, and partial results.

## Project status

Arphix Agents is currently experimental. Planned milestones include:

- [ ] Define stable agent, tool, memory, and event interfaces.
- [ ] Add a local single-agent runner.
- [ ] Add structured tool-call and execution logs.
- [ ] Add configurable model providers.
- [ ] Add multi-agent orchestration.
- [ ] Add memory adapters with retention controls.
- [ ] Add evaluation datasets and regression tests.
- [ ] Add approval checkpoints.
- [ ] Publish versioned documentation and examples.

## Contributing

Contributions are welcome. For larger changes, open an issue before implementation so the design can be discussed. Keep pull requests focused, add tests for changed behavior, document new configuration, and run the formatter and test suite before submitting.

Do not include API keys, private data, production logs, or unreviewed agent traces in issues or pull requests.

## License

Arphix Agents is intended to be released under the [MIT License](LICENSE). Add the complete license text to `LICENSE` before publishing.

## Logo setup

Save the provided Arphix logo image in the repository at:

```text
assets/arphix-agents-logo.png
```

The logo is displayed at the top of this README. If your asset uses a different filename, update the image path in the first section.

## Support

Use GitHub Issues for bug reports and feature requests. Include the project version, environment, configuration summary, reproduction steps, and relevant logs with secrets removed.

<!--
Badge customization:
- Replace <your-username>/<your-repository> with the real GitHub repository.
- Replace static status and test badges with live GitHub Actions badges when CI is configured.
- Add provider-specific badges only when those providers are actually supported.
-->

