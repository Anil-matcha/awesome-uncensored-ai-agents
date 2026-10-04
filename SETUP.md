# General-purpose agent setup

These examples use Muapi's OpenAI-compatible Chat Completions API. Keep `MUAPI_API_KEY` in the environment of the agent process, not in a committed config file. First inspect the live model list and select a model with `capabilities.tools: true`:

```bash
export MUAPI_API_KEY="your-muapi-api-key"
curl -s "https://api.muapi.ai/v1/models?type=text" \
  -H "Authorization: Bearer $MUAPI_API_KEY"
```

Model IDs and tool support can change. Use the exact live ID in the configuration below.

## OpenClaw

OpenClaw supports a custom OpenAI-compatible model provider. Add a provider and default model to `~/.openclaw/openclaw.json`:

```json5
{
  models: {
    mode: "merge",
    providers: {
      muapi: {
        baseUrl: "https://api.muapi.ai/v1",
        apiKey: "${MUAPI_API_KEY}",
        api: "openai-completions",
        models: [
          {
            id: "YOUR_TOOL_CAPABLE_MODEL_ID",
            name: "Muapi text model",
          },
        ],
      },
    },
  },
  agents: {
    defaults: {
      model: {
        primary: "muapi/YOUR_TOOL_CAPABLE_MODEL_ID",
      },
    },
  },
}
```

Replace the placeholder in both locations. Keep the key in the gateway's service environment when OpenClaw runs as a background service. Check available models with `openclaw models list`, then test a read-only task from its terminal UI before connecting messaging channels.

See the [Muapi OpenClaw guide](https://muapi.ai/docs/openclaw), [OpenClaw custom provider documentation](https://docs.openclaw.ai/gateway/config-tools/custom-providers), and [OpenClaw model configuration](https://docs.openclaw.ai/gateway/config-agents/models).

## Hermes Agent

Hermes Agent can use a custom OpenAI-compatible endpoint. Run its setup wizard:

```bash
hermes model
```

Choose **Custom endpoint** and set the API base URL to `https://api.muapi.ai/v1`, the API key to your Muapi key, and the model name to an exact live text model ID with tool support.

Alternatively, edit `~/.hermes/config.yaml`:

```yaml
model:
  default: YOUR_TOOL_CAPABLE_MODEL_ID
  provider: custom
  base_url: https://api.muapi.ai/v1
  key_env: MUAPI_API_KEY
```

Store the key in `~/.hermes/.env` with restricted file permissions. Check the current Hermes context window requirement and the model's supported context before use. Test a simple, read-only task before enabling gateway channels or other tools.

See the [Muapi Hermes guide](https://muapi.ai/docs/hermes-agent), [Hermes provider documentation](https://hermes-agent.nousresearch.com/docs/integrations/providers), and [Hermes configuration guide](https://hermes-agent.nousresearch.com/docs/user-guide/configuration).

## Tool-enabled smoke task

Use a disposable workspace with a harmless text file and ask the agent to read it and summarize its contents. Do not grant write or shell access for this first connection check. Then, if the agent's permissions and environment are correctly isolated, allow one bounded write task in a throwaway directory and inspect the resulting diff.

Record the agent version, model ID, model capabilities, task, tool calls, result, elapsed time, and dashboard usage. A successful API response alone does not prove that the agent performed a tool call.
