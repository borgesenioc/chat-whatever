# CLAUDE.md

## Canonical Rules

These apply to all Claude interactions in this project:

1. **No emojis** in code, commits, or documentation
2. **Never commit or push** - user handles git operations
3. **Write for impatient senior engineers** - no verbose explanations, no AI filler, no disclaimers
4. **Lean docs** - if it can be said in 5 words, don't use 20
5. **No over-engineering** - solve the problem at hand, not hypothetical future ones

## Project Overview

WhatsApp AI chatbot for Brazilian users. Zero-friction entry (click and chat), free LLM backend, context retention.

**Stack**: TypeScript, Node, Postgres, Redis (BullMQ), Vercel AI SDK

## Architecture

See `docs/ARCHITECTURE.md` for diagrams.

```
User -> WhatsApp -> Webhook API -> Queue -> Worker -> Agent Orchestrator -> LLM
                                                   -> Context Engine (Postgres)
```

**Key components**:
- Webhook API: receives WhatsApp, returns 200 fast
- Queue: BullMQ on Redis
- Worker: consumes jobs, coordinates flow
- Agent Orchestrator: tool loop via Vercel AI SDK
- Context Engine: state + summaries + prompt builder

## Dev Commands

```bash
# TBD - project not yet initialized
```

## Key Files

```
src/
  api/          # Webhook handlers
  worker/       # Job consumers
  agent/        # Orchestrator + tools
  context/      # State management, summaries
  db/           # Postgres models
```

## Context System

Per conversation (per phone number):
- `summary`: rolling conversation summary
- `last_messages`: raw last 10-20 messages
- `user_prefs`: language, tone, constraints
- `active_goal`: current intent (one sentence)
- `facts`: key-value memory (optional)

## External Docs

- [Vercel AI SDK](https://sdk.vercel.ai/docs)
- [OpenRouter](https://openrouter.ai/docs)
- [WhatsApp Cloud API](https://developers.facebook.com/docs/whatsapp/cloud-api)
