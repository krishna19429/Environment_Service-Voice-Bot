
# 09 — Happy Path 

 

## Overview 

 

Complete end-to-end call flow with no errors — from greeting to goodbye. 



### Demo Customer Record 
|Field|Value|
|-------|-------| 
|Customer ID |C001 | 
|Phone Number |9876543210 | 
|Service Address |Baner, Pune | 
|Service Date |2026-06-20 | 
|Status |Scheduled | 

 

--- 

 

## Step-by-Step: Missed Pickup Report 

 

|Step |Speaker |Dialogue |System Event | 
|------|---------|----------|--------------| 
|1 |Gateway |(Call connects) |STT activated | 
|2 |Bot |Thank you for calling Waste Management Services. I am your automated voice assistant. How can I help you today? |Root Agent entry fulfillment | 
|3 | Customer | My garbage was not picked up this morning. | STT → NLU | 
| 4 | System | (Intent classified) | service.missed_pickup at 94% confidence | 
| 5 | Bot | I can help with that. Please say your 10-digit registered phone number. | Route to Auth Agent | 
| 6 | Customer | 9876543210 | @sys.phone-number entity captures | 
| 7 | Bot | Thank you. And your service address? | phone_number filled | 
| 8 | Customer | Baner, Pune | @sys.any captures address | 
| 9 | System | (Webhook) | validateAddress(9876543210, Baner Pune) → verified=true | 
| 10 | Bot | Thank you, Ankit. You are verified. | Session: is_authenticated=true, customer_id=C001 | 
| 11 | System | (Route to Missed Pickup Agent) | customer_id passed via session | 
| 12 | Bot | You are reporting a missed pickup. Was it missed today, June 20th? | Missed Pickup Agent entry | 
| 13 | Customer | Yes, today. | @sys.date resolves to 2026-06-20 | 
| 14 | System | (Webhook) | reportMissedPickup(C001, 2026-06-20) → ticket MP-C001-20260620 | 
| 15 | Bot | Logged. Team will attend tomorrow June 21st. Ticket: MP-C001-20260620. Anything else? | Response fulfillment | 
| 16 | Customer | No, thank you. | | 
| 17 | Bot | Thank you for calling. Have a great day. Goodbye! | End session | 
