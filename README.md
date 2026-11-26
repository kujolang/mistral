# Kujo Mistral Provider

[![Version](https://img.shields.io/badge/version-0.1.1-black)](https://github.com/kujolang/mistral/releases/tag/v0.1.1)
[![License](https://img.shields.io/badge/license-MIT-lightgrey)](LICENSE)
[![built with Kujo](https://img.shields.io/badge/built%20with-Kujo-white.svg)](https://github.com/kujolang/kujo)

Mistral chat, embeddings, and provider-native AI services for Kujo.

## Install

The current source requires Kujo 1.6.0 or later. Kennel resolves the AI SDK revision pinned in the package manifest. The commands below install the published v0.1.1 release.

```bash
kujo run /path/to/kennel/kennel.kujo \
  -- add github:kujolang/mistral@v0.1.1 \
  --alias mistral
kujo run /path/to/kennel/kennel.kujo -- install
export MISTRAL_API_KEY=your-key
```

## 30-second quick start

```kujo
from mistral import create_client, client_chat

client := create_client({})
request := {
    "model": "mistral-large-latest",
    "messages": [
        {
            "role": "user",
            "content": "Hello from Kujo!"
        }
    ]
}

result := client_chat(client, request)

print(result["data"]["choices"][0]["message"]["content"])
```

## Native API

The native layer preserves Mistral chat responses, tools, structured output, reasoning, model metadata, usage, and embeddings. OCR, files, agents, audio, and batch workflows remain provider-owned.

## AI SDK integration

`mistral_provider({"model": "mistral-large-latest"})` supplies normalized chat and streaming semantics through the compatible driver.

## Authentication and security

Set `MISTRAL_API_KEY`. Configure a trusted HTTPS base URL when using a gateway. Models, chat, and embeddings validate bearer credentials before sending requests and disable redirects. Native responses redact the configured credential, including JSON-escaped echoes.

## Testing and documentation

```bash
bash scripts/release_quality_gate.sh
bash scripts/verify_installed_package.sh
```

The release gate uses deterministic offline fixtures. The installed-package smoke checks the published tag by default. Set `MISTRAL_REF` to a full commit SHA to check that revision. See [docs/](docs/) for implementation and Contract v1 evidence.
