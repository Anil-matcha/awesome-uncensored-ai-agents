# Awesome Uncensored AI Agents

A practical guide to connecting tool-capable, low-refusal language models to general-purpose AI agent stacks. This repository focuses on agents that work across a terminal, workspace, or messaging channels, such as OpenClaw and Hermes Agent. It uses Muapi-hosted models for the provider examples and keeps agent safety and permissions visible.

“Uncensored” is a community label, not a standardized model property or a guarantee. This guide does not claim every model is tool-capable or that reduced refusals imply better task quality.

## Related Projects

- [uncensored-coding-models](https://github.com/Anil-matcha/uncensored-coding-models) — coding-agent benchmark and setup guides for Codex CLI, Claude Code, and OpenCode.
- [awesome-uncensored-llms](https://github.com/Anil-matcha/awesome-uncensored-llms) — broader language-model catalog with provenance notes.
- [awesome-openclaw](https://github.com/Anil-matcha/awesome-openclaw) — general OpenClaw resources, skills, and integrations.
- [awesome-hermes-agent](https://github.com/Anil-matcha/awesome-hermes-agent) — general Hermes Agent skills, plugins, and integrations.
- [Muapi Abliterated LLM API](https://muapi.ai/abliterated-llm-api) — hosted text models and current tool-capability information.
- [Muapi agent guides](https://muapi.ai/docs/agents) — setup and protocol documentation.
- [Muapi dashboard](https://muapi.ai/dashboard) — create API keys and review usage.
- [Muapi](https://muapi.ai) — hosted model APIs.

## What this covers

- General-purpose agent stacks that call tools on a user's behalf.
- Exact provider URL, protocol, authentication, and model-list discovery steps.
- The difference between model tool-call support and an agent stack's own tool permissions.
- Workspace isolation, confirmation gates, credential handling, and credit monitoring.

It does not duplicate model catalogs or coding-agent setup. For Codex CLI, Claude Code, and OpenCode, use [uncensored-coding-models](https://github.com/Anil-matcha/uncensored-coding-models). For broader framework-specific skills and deployment resources, see the related OpenClaw and Hermes catalogs above.

## Compatibility snapshot

| Agent stack | Connection path | Muapi API | State |
|---|---|---|---|
| OpenClaw | Custom OpenAI-compatible provider | `https://api.muapi.ai/v1` | Setup documented; choose a live model with `capabilities.tools: true` |
| Hermes Agent | Custom OpenAI-compatible endpoint | `https://api.muapi.ai/v1` | Setup documented; check the model context window against Hermes' current minimum |

Model availability and capabilities can change. Check the live text model list before configuring an agent:

```bash
curl -s "https://api.muapi.ai/v1/models?type=text" \
  -H "Authorization: Bearer $MUAPI_API_KEY"
```

Choose a model whose entry has `capabilities.tools: true`. For Hermes, also confirm its context requirement against current Hermes documentation. No agent/model benchmark results are published here yet.

## Get started

1. Create a Muapi key in the [dashboard](https://muapi.ai/dashboard), then store it in the environment used by the agent process.
2. Check the live model list and select a text model that advertises tool support.
3. Follow the provider setup in [`SETUP.md`](SETUP.md) for OpenClaw or Hermes Agent.
4. Start with a read-only task in a disposable workspace. Grant only the tools the agent needs.
5. Add channels and persistent workflows after the tool flow and credit usage are understood.

Each model request uses credits. Long conversations can cost more because the agent may resend its chat history. Check current model rates and usage in the Muapi dashboard before enabling a long-running gateway.

## Safety baseline

An agent can take actions through tools even when the model only returns text. Run it in a dedicated workspace, avoid mounting personal or production data, keep destructive tools confirmation-gated, and do not expose an unauthenticated agent gateway to a network. Store API keys outside shared configuration. See [`SECURITY.md`](SECURITY.md) for a setup checklist.

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md). Keep setup steps tied to current primary docs, qualify compatibility claims, and record a real end-to-end run before labeling a stack/model pair tested.

## License

Original documentation and configuration examples are licensed under MIT. See [`LICENSE`](LICENSE). Agent frameworks, model services, and linked resources retain their own licenses and terms.
