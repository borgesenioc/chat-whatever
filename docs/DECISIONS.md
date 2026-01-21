# Decisions

Architecture Decision Records. Why we chose what we chose.

---

## 001: TypeScript over Python

**Context**: Need webhook handling, queue processing, LLM calls.

**Decision**: TypeScript/Node

**Rationale**:
- WhatsApp webhooks + HTTP concurrency: Node excels
- LLM calls are I/O-bound: perfect for Node
- One language for webhook + worker + context engine
- BullMQ ecosystem is mature
- Can add Python workers later for ML-specific tasks

---

## 002: Vercel AI SDK over LangChain

**Context**: Need agent orchestration with tool calling.

**Decision**: Vercel AI SDK

**Rationale**:
- Cleaner API, less abstraction overhead
- Model-agnostic out of the box
- `generateText` + `tools` + `maxSteps` is exactly what we need
- LangChain is over-engineered for this use case

---

## 003: BullMQ over direct processing

**Context**: WhatsApp webhooks need fast 200 response.

**Decision**: Queue with BullMQ on Redis

**Rationale**:
- Decouple receipt from processing
- Built-in retries, concurrency limits
- Scheduled jobs (summarize every N messages)
- Dead-letter handling
- Scale later to SQS if needed, but not now

---

## 004: Rolling summaries over full history

**Context**: Need context retention without token explosion.

**Decision**: Store rolling summary + last N messages

**Rationale**:
- Keep token budget predictable
- Summarize every 8-12 messages automatically
- Store raw messages for debugging, but don't send all to LLM
- Cheaper than vector search for this use case

---

## 005: OpenRouter over direct API

**Context**: Need LLM provider.

**Decision**: OpenRouter

**Rationale**:
- Free models available (critical for R$1-3/month pricing)
- Model switching without code changes
- Single API for multiple providers
