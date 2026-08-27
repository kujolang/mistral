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

Kujo >= 1.0.2; `github:kujolang/ai-sdk@v1.1.0`.

## Authentication / Native Semantics / Streaming

Bearer `MISTRAL_API_KEY`, HTTPS enforcement, URL credential rejection, redaction, protected headers, and OpenAI-compatible SSE parsing. Native response fields remain in raw provider data.

## Tools / Structured Output / Reasoning / Multimodal / Embeddings

Tools, response format, reasoning, and multimodal request fields remain provider-owned. Vision is declared where model-supported. Embeddings are not claimed.

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
