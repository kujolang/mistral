# Mistral Implementation Report

## Executive Summary

Initial Kujo Mistral package for the documented Mistral OpenAI-compatible HTTP API, with a provider-qualified native client and pure AI SDK driver.

## Official API Evidence / Evidence Date

Mistral's official SDK documents `MISTRAL_API_KEY`, clients, chat, function calling, structured outputs, embeddings, OCR, files, and model-specific controls. The API uses `https://api.mistral.ai/v1`, bearer authentication, chat-completions-compatible requests, server-sent streaming, and native OCR/document endpoints. Evidence date: 2026-08-27. Sources: official [client SDK](https://github.com/mistralai/client-python), [chat API](https://docs.mistral.ai/api), [embeddings API](https://docs.mistral.ai/api/endpoint/embeddings), and [OCR API](https://docs.mistral.ai/api/endpoint/ocr).

## Protocol Classification

OPENAI-COMPATIBLE WITH PROVIDER EXTENSIONS. Mistral provides chat-completions-compatible wire semantics while adding native embeddings, OCR/document processing, agents, files, and Mistral-specific metadata.

## Architecture / Native API Coverage

Native client: `src/mistral.kujo`; AI SDK adapter: `src/provider.kujo`; root exports: `mistral.kujo`. Chat, model listing, embeddings, SSE parsing, tools, reasoning options, and usage are covered.

## Public Exports

`create_client`, `chat`, `client_chat`, `client_models`, `client_embeddings`, `embeddings`, `parse_stream`, `mistral_provider`, `mistral_driver`.

## Kujo Requirement / AI SDK Dependency

The current source requires Kujo 1.6.0 or later. Kennel pins AI SDK commit `f3126dded28102f40d0ddecd50a559105c66f905`.

## Authentication / Native Semantics / Streaming

Native requests use bearer `MISTRAL_API_KEY`, validate HTTPS endpoints and credentials, disable redirects, and cap response bodies at 8 MiB. The SDK driver protects Authorization and Content-Type and restricts requests to the configured origin.

The SSE parser handles split transport chunks and CRLF framing. The driver requires a terminal finish reason and the documented `[DONE]` marker. Native response fields remain in raw provider data after credential redaction. [Mistral Chat API](https://docs.mistral.ai/api/endpoint/chat)

## Tools / Structured Output / Reasoning / Multimodal / Embeddings

The driver preserves full tool turns and typed message parts, forwards `reasoning_effort`, and retains native thinking chunks in the raw result. Vision is available on supported models. Embeddings are exposed through the native client. [Mistral reasoning guide](https://docs.mistral.ai/inference/conversations/reasoning)

## Usage / Finish Reasons / Errors

Prompt/completion/total usage maps where supplied. Native error payloads and provider codes are retained subject to redaction.

## AI SDK Driver / Security / Tests

Pure descriptor/decoder hooks with no network I/O or policy bypass. Two deterministic offline files plus installed consumer smoke are included.

## Clean-Room Install / Installed Consumer Smoke

Passed with Kujo v1.0.2, including immutable Kennel add/install/reinstall/validate and installed consumer smoke with `KUJO_MODULE_PATH` unset.

## Live Validation

SKIPPED — credentials/environment unavailable.

## AI SDK Changes / Kujo Changes / Kennel Changes

None. Embeddings are exposed natively; the normalized AI SDK driver claims chat capabilities only because paired embedding hooks are not implemented.

## Contract Conformance / Limitations

See `MISTRAL_PROVIDER_PACKAGE_CONFORMANCE.md`. Native gRPC SDK semantics, image/video generation endpoints, files, batch jobs, and realtime features are outside this initial HTTP package.
