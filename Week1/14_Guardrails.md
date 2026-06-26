# 14 — Guardrails 

 

## Authentication Guardrails 

 

- `is_authenticated` check at the START of every Sub-Agent — if false, transfer to Auth Agent 

- Customer data is NEVER accessible without `is_authenticated = true` in session 

- Auth session params (customer_id, phone, address) are read-only after being set 

- Three-strike policy is absolute — no override path 

 

--- 

 

## PII Handling 

 

| PII Type | How Handled | 
|----------|------------| 
| Phone number | Captured via @sys.phone-number. Not repeated aloud in full. Partial masking in confirmation. | 
| Service address | Stored in session. Used for operations but not verbosely repeated. | 
| Customer name | Used for personalization only — not stored beyond session. | 
| Ticket IDs | Spoken in full — these are the caller's reference numbers. | 
| Conversation transcripts | Stored in CX history. Access controlled via GCP IAM. | 

 

--- 

 

## Hallucination Prevention 

 

- Bot uses ONLY data returned by API calls — never fabricates ticket IDs, dates, or ETAs 

- Bot cannot diagnose root cause of service failures — logs and escalates only 

- Bot cannot make pricing, contract, or account promises outside tool capabilities 

- If API returns no data: say 'I was unable to find that information' — never generate plausible-sounding false data 

 

--- 

 

## Scope Guardrails 

 

**In scope:** missed pickup, reschedule, status, route delay, blockage, complaint, next pickup 

 

**Out of scope:** billing, payments, account creation, contract changes, legal matters 

 

Out-of-scope handling: acknowledge, explain scope, route to Escalation Agent. Do not partially handle. 

 

--- 

 

## Escalation Boundaries 

 

- Escalation is irreversible — bot does not re-engage same caller after transfer 

- Full session context always passed to human agent 

- Estimated wait time provided if available — never fabricated 

- Escalation triggers are absolute — no LLM discretion on whether to escalate
