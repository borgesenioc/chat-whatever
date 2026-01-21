# Domain Knowledge

Business logic and external constraints that code can't capture.

## Target User

Brazilian, low digital literacy, lives on WhatsApp. Can't/won't:
- Download new apps
- Create accounts
- Type long messages
- Navigate complex UIs

## WhatsApp Constraints

- No streaming support (blocking responses only)
- Webhooks may retry - need idempotency keys
- 24-hour messaging window rules
- Rate limits apply
- Media handling has size limits

## Business Model

- Freemium: R$1-3/month
- Viral growth via user recommendations
- Users get hooked before realizing it's AI

## Bot Personas

10-12 specialized bots trained for friendly conversation (not just support):
- Each bot has distinct personality/specialty
- Conversation tone, not transactional
- Context retention across sessions

## Key Metrics (planned)

- Time to first response
- Conversation length
- Return rate
- Conversion to paid

## Localization

- Brazilian Portuguese
- Informal tone appropriate for WhatsApp
- Handle voice messages (future)
