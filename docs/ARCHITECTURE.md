# Architecture

## System Flow

```
WhatsApp -> /webhook POST -> store message -> enqueue job -> return 200
                                                   |
                                                   v
                                              Worker picks up
                                                   |
                                                   v
                                         Load context (summary + recent + prefs)
                                                   |
                                                   v
                                         Agent Orchestrator loop
                                                   |
                                                   v
                                         Persist reply + update summary
                                                   |
                                                   v
                                         Send via WhatsApp API
```

## Components

| Component | Responsibility |
|-----------|---------------|
| Webhook API | Receive WhatsApp webhooks, validate, enqueue, return 200 fast |
| Queue (BullMQ) | Job management, retries, concurrency limits, scheduled jobs |
| Worker | Consume jobs, load context, call orchestrator, persist, send reply |
| Agent Orchestrator | Tool loop via Vercel AI SDK (`generateText` + `tools` + `maxSteps`) |
| Context Engine | Build prompts from summary + recent + prefs + active goal |
| Postgres | Messages, summaries, user state, facts |
| Redis | Queue backend, optional caching |

## Agent Loop

```
loop until final response:
    prompt + tools -> LLM -> response
    if tool_call:
        execute tool
        append result to context
    else:
        return final reply
```

## Prompt Structure

1. **System**: rules, safety, "you are a WhatsApp assistant"
2. **Developer**: bot behavior (tone, boundaries)
3. **Context**: summary + active goal + prefs
4. **Recent**: last N turns
5. **User**: current message

## Hosting (planned)

- App/Worker: Cloud Run or AWS ECS/Fargate
- DB: Neon/Supabase (RDS later)
- Redis: Upstash
