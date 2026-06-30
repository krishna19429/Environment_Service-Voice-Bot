# 11 — Voice Experience 

 

## Voice Persona 

 

| Attribute | Definition | 
|-----------|------------| 
| Identity | Automated Waste Management assistant — no explicit name | 
| Tone | Professional, warm, calm | 
| Language | Simple English, Indian context, no jargon | 
| Empathy | Expressed for complaints and delays | 
| Conciseness | Max 2-3 sentences per turn | 

 

--- 

 

## SSML Examples 

 

```xml 

<!-- Read ticket ID character by character --> 

<speak>Your ticket number is 

  <say-as interpret-as='characters'>MP-C001-20260620</say-as> 

</speak> 

 

<!-- Pause after important info --> 

<speak>Your pickup is rescheduled to Monday June 23rd. 

  <break time='500ms'/> Is there anything else? 

</speak> 

 

<!-- Read phone number digit by digit --> 

<speak>Confirming <say-as interpret-as='telephone'>9876543210</say-as></speak> 

 

<!-- Emphasize a date --> 

<speak>Our team will attend 

  <emphasis level='moderate'>tomorrow, June 21st</emphasis>. 

</speak> 

``` 

 

--- 

 

## Barge-In Handling 

 

- Barge-in is enabled by default on Google Phone Gateway 

- When caller interrupts: bot stops immediately and listens 

- Keep bot messages short to reduce need for barge-in 

- Long menus: state key options first 

 

--- 

 

## Latency Targets 

 

| Operation | Target | 
|-----------|--------| 
| NLU intent classification | < 300ms | 
| Webhook warm | < 1 second | 
| Webhook cold start | < 3 seconds | 
| Full turn (speech to response) | < 5 seconds | 
| TTS synthesis | < 500ms | 

 

--- 

 

## Confirmation Strategy 

 

| Operation | Confirmation Required | 
|-----------|----------------------| 
| Service status check | No — read-only | 
| Route delay check | No — read-only | 
| Next pickup date | No — read-only | 
| Report missed pickup | Date confirmation only | 
| Reschedule pickup | Full yes/no confirmation required | 
| Container blockage | No — always appropriate to log | 
| Formal complaint | No — proceed immediately | 

 
