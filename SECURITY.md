# Agent setup safety checklist

General-purpose agents can call tools that read files, run commands, browse pages, or send messages. Apply these controls before connecting one to a real workspace or channel.

- **Isolate the workspace.** Use a dedicated directory or disposable container. Do not mount personal files, production credentials, or unrelated repositories.
- **Start read-only.** Confirm model selection and API connectivity with a read-only task. Add write or command tools only when required.
- **Require confirmation.** Keep deletion, shell commands, purchases, posts, messages, and external account changes behind human approval.
- **Protect the key.** Use a restricted environment file or secret store. Never commit, log, screenshot, or include the key in a prompt.
- **Limit network access.** Do not expose an agent gateway publicly without authentication. Restrict inbound channels and tool access to intended users.
- **Set a spend boundary.** Check current model prices, credit balance, and usage. Long sessions can resend conversation history and consume credits.
- **Treat content as untrusted.** Files, web pages, and messages can contain instructions. Do not let tool output silently expand an agent's permissions.
- **Review actions.** Inspect diffs, terminal commands, sent messages, and other external side effects before accepting them.

Tool-capable models and safe agents are different things: `capabilities.tools: true` means the model can return tool calls. It does not grant permission or make a tool action safe.
