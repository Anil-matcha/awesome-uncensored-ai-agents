# Awesome Uncensored AI Agents

A practical guide to connecting low-refusal language models to general-purpose AI agent stacks. It focuses on agents that can use tools across a terminal, workspace, or messaging channels, with setup examples for OpenClaw and Hermes Agent.

“Uncensored” is a community label, not a standardized model property or a guarantee. Reduced refusals do not establish quality, reliability, privacy, or suitability for a task. A model’s ability to return tool calls is separate from the agent’s permissions to execute them.

## Contents

- [What you can use these agents for](#what-you-can-use-these-agents-for)
- [Supported integrations](#supported-integrations)
- [Choose a model and agent](#choose-a-model-and-agent)
- [Quick start](#quick-start)
- [Smoke test](#smoke-test-before-adding-tools)
- [Safety baseline](#safety-baseline)
- [Evaluation notes](#evaluation-notes)

## What you can use these agents for

General-purpose agents can combine a model with tools, memory, and optionally messaging channels. Start with a small, bounded workflow and grant only the tools it needs.

| Use case | Example task | Start with |
|---|---|---|
| **Personal knowledge assistant** | Search a small set of notes and summarize what relates to a question. | Read-only file access to a dedicated notes folder. |
| **Research and synthesis** | Gather information from approved sources and return a sourced comparison. | Browser/search tools with explicit source and citation requirements. |
| **Workspace helper** | Create a draft, organize project files, or update a local checklist. | A disposable workspace and confirmation before writes. |
| **Scheduled or channel-based assistant** | Respond to messages or run recurring tasks through a connected channel. | First validate the gateway privately; then connect only intended users and channels. |
| **Tool integration prototyping** | Test whether an exact model and provider endpoint can complete a tool round trip. | The read-only smoke test below, followed by one harmless bounded write. |
| **Long-running agent workflow** | Chain research, analysis, and a draft across multiple turns. | Spend limits, turn limits, isolated credentials, and human review before external actions. |

For code edits through IDE or terminal coding agents, see [Awesome Uncensored Coding Models](https://github.com/Anil-matcha/awesome-uncensored-coding-models). This guide covers general-purpose agent stacks rather than coding-agent benchmarks.

## Supported integrations

These setup paths use Muapi’s OpenAI-compatible Chat Completions API. The status describes documentation only; this repository has not published agent/model benchmark results.

| Agent stack | Connection path | Setup guide | Current status |
|---|---|---|---|
| OpenClaw | Custom OpenAI-compatible provider at `https://api.muapi.ai/v1` | [OpenClaw setup](SETUP.md#openclaw) | Setup documented; verify a live tool-capable model and test the tool round trip. |
| Hermes Agent | Custom OpenAI-compatible endpoint at `https://api.muapi.ai/v1` | [Hermes setup](SETUP.md#hermes-agent) | Setup documented; check the model context against Hermes’ current requirement. |

Model IDs and capabilities can change. Ask the live model endpoint instead of copying an old ID:

```bash
curl -s "https://api.muapi.ai/v1/models?type=text" \
  -H "Authorization: Bearer $MUAPI_API_KEY"
```

Select a text model whose response has `capabilities.tools: true`. For Hermes, also check the current minimum context requirement against the model’s supported context. The `tools` flag means the model can return tool calls; the agent runtime still decides which tools are available and whether actions require confirmation.

## Choose a model and agent

Pick the integration by where the agent should work: OpenClaw offers a general agent gateway and channel-oriented setup; Hermes Agent offers its own CLI, configuration, and provider system. Consult the linked primary docs for current features and configuration options before installing either stack.

Then choose a model based on the workflow:

1. **For chat or summarization without actions,** tool calling is optional. Use a text model and check answer quality on representative prompts.
2. **For agents that call tools,** require `capabilities.tools: true` on the exact model and API route. Verify at least one real tool call, not just a successful text response.
3. **For image-based requests,** check vision support at the model, endpoint, and agent layers. Do not assume the general text endpoint passes images through.
4. **For long tasks,** check current context limits, rate limits, pricing, and how the agent stores or resends conversation history.
5. **For privacy-sensitive work,** review the provider’s current data and retention terms. An agent running locally does not make a remote model endpoint local.

No model is ranked here. Reduced refusal behavior, model size, and a successful API response do not prove that an agent will finish a task accurately or use tools reliably. See the [Abliterated LLM API catalog](https://muapi.ai/abliterated-llm-api) for hosted variants and the [broader model catalog](https://github.com/Anil-matcha/awesome-uncensored-llms) for provenance notes.

## Quick start

1. Create a key in the [Muapi dashboard](https://muapi.ai/dashboard).
2. Export it into the environment where the agent process will run:

   ```bash
   export MUAPI_API_KEY="your-muapi-api-key"
   ```

3. Query the live model list and choose an exact text ID with `capabilities.tools: true`.
4. Follow the provider configuration in [SETUP.md](SETUP.md).
5. Run the read-only smoke test below before granting write, shell, or messaging tools.

Each request uses credits. Long conversations may resend prior history and use more tokens. Check current model prices and dashboard usage before running persistent agents.

## Smoke test before adding tools

Use a private, disposable workspace with one harmless text file. Ask the agent only to read the file and summarize it. For this first check, do not grant write, shell, browser, or external messaging tools. Confirm that the agent is connected to the expected model and returns a grounded summary.

If it passes, allow one bounded file-creation task inside the disposable workspace. Inspect the tool calls and output yourself. Record the agent version, model ID, `tools` capability, task, endpoint, tool calls, result, elapsed time, and usage. A successful HTTP response alone does not demonstrate tool use.

## Safety baseline

An agent can act through tools even when its model only produces text. Use a dedicated workspace, avoid mounting personal or production data, keep destructive actions confirmation-gated, and do not expose an unauthenticated agent gateway to a network.

- Store keys in a restricted environment file or secret store, never in committed configuration.
- Limit connected channels and users to the intended audience.
- Treat files, pages, and messages as untrusted input; they can contain prompt-injection attempts.
- Review commands, file diffs, messages, and other external side effects before accepting them.
- Set usage boundaries and monitor credits for persistent agents.

See the detailed [setup safety checklist](SECURITY.md). Tool-capable models and safe agent deployments are different things: `capabilities.tools: true` grants no permission by itself.

## Evaluation notes

Keep connection testing separate from task-quality evaluation. To compare model/agent combinations, use the same starting workspace, task, permissions, agent version, and endpoint protocol. Repeat each task and record whether the model selected the right tools, followed constraints, recovered from errors, and produced the expected result. Include failed or incomplete runs; do not label an integration “tested” based only on configuration or a text reply.

## Related projects

- [Awesome Uncensored Coding Models](https://github.com/Anil-matcha/awesome-uncensored-coding-models) — coding-model candidates, benchmark runner, and Codex CLI / Claude Code / OpenCode setup.
- [Awesome Uncensored LLMs](https://github.com/Anil-matcha/awesome-uncensored-llms) — broader language-model catalog with provenance notes.
- [Awesome Abliterated LLMs](https://github.com/Anil-matcha/awesome-abliterated-llms) — Muapi hosted endpoint guide and API examples.
- [Awesome Uncensored AI Models](https://github.com/Anil-matcha/awesome-uncensored-ai-models) — index to the language, image, and video model catalogs.
- [Awesome OpenClaw](https://github.com/Anil-matcha/awesome-openclaw) — general OpenClaw resources and integrations.
- [Awesome Hermes Agent](https://github.com/Anil-matcha/awesome-hermes-agent) — general Hermes Agent resources and integrations.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Link current primary setup docs, identify the exact protocol and model ID, and label an integration as documented until a real end-to-end tool round trip has been recorded. Keep safety controls and usage costs visible; never include real API keys or fabricated success claims.

## License

Original documentation and configuration examples are licensed under MIT. See [LICENSE](LICENSE). Agent frameworks, model services, and linked resources retain their own licenses and terms.
