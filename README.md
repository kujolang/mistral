# Kujo Mistral Provider

Native Mistral OpenAI-compatible chat client with Mistral reasoning, tools, vision, and an AI SDK adapter.

```bash
kujo package-add github:kujolang/mistral@v0.1.0
export MISTRAL_API_KEY=your-key
```

```kujo
from mistral import create_client, client_chat
c := create_client({})
r := client_chat(c, {"model":"mistral-large-latest","messages":[{"role":"user","content":"Hello"}],"reasoning_effort":"high"})
```

Native use preserves Mistral response fields, reasoning controls, tools, and usage metadata. `mistral_provider()` supplies normalized AI SDK chat and streaming semantics. Tests are offline and credential-free.
