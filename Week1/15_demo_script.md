# 15 — Demo Script 

 

## Week 1 Presentation Flow (10 minutes) 

 

| Time | Section | What to Say / Do | 
|------|---------|-----------------| 
| 0:00-1:00 | Opening | 'We built a voice bot for waste management. It handles 7 service types, 24/7, using CX Agent Studio multi-agent architecture. Let me show you live first.' | 
| 1:00-3:30 | Live Demo | Call Phone Gateway. Auth with C001. Report missed pickup. Show ticket number. | 
| 3:30-5:00 | Architecture | Show diagram. Explain: Root Agent → Auth Agent → 7 Sub-Agents → Cloud Functions. | 
| 5:00-7:00 | Business Value | '70% containment target. AHT under 3 min vs 10-15 min manual. 24/7 availability.' | 
| 7:00-9:00 | Week 2 Plan | 'Edge cases, auth failure retry, escalation with context, full test suite.' | 
| 9:00-10:00 | Q&A | Handle mentor questions — see below. | 

 

--- 

 

## Expected Questions and Answers 

 

**Q: Why CX Flows instead of Playbooks?** 

> Flows are deterministic — every demo run is identical. Playbooks are non-deterministic. 

> For graded presentations, reliability matters more than LLM flexibility. 

> We layer Playbooks for free-form complaint description in Week 2. 

 

**Q: How does authentication work technically?** 

> Caller provides phone + address. CX captures them as session params using @sys.phone-number 

> and @sys.any entities. Auth Agent calls validateAddress() — a Python Cloud Function — 

> which queries our database and returns verified=true with customer_id. 

 

**Q: How would this scale to 10,000 concurrent calls?** 

> Dialogflow CX handles unlimited concurrent sessions natively. 

> Cloud Functions auto-scale horizontally — each call gets its own compute instance. 

> The only bottleneck in production is the CRM — replaced with a load-balanced connection pool. 

 

**Q: How do you handle PII in transcripts?** 

> Phone numbers use partial masking. Names used for personalization only. 

> In production: GCP IAM controls transcript access. Cloud DLP enables PII masking in logs. 

 

**Q: What happens when the webhook times out mid-call?** 

> Cloud Function has 10-second timeout. CX webhook timeout set to 5 seconds. 

> try/except block returns user-friendly fallback: 'Technical issue. Connecting you to agent.' 

> Escalation Agent receives full session context — caller does not repeat information. 

 

**Q: What is your containment rate and how do you measure it?** 

> Containment rate = % of calls fully resolved without human escalation. Target: >70%. 

> Measured via CX conversation history — any session reaching Escalation Agent = uncontained. 

> CCAI Insights provides this metric automatically in Week 3. 

 
