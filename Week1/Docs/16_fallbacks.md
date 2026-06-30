# FALLBACKS

FALLBACKS

## 1. Root Agent Fallback 

When to trigger
User's intent is unclear.
User asks something unsupported.
Agent cannot determine which sub-agent to route to.

### Prompt

I'm sorry, I didn't understand your request.

I can help you with:
• Check pickup status
• Report a missed pickup
• Reschedule a pickup
• Check route delays
• Report a blocked container
• Register a complaint

Could you please rephrase your request?
After 2 failures
I'm still having trouble understanding your request.

I'll connect you with a customer support representative who can assist you further.

## 2. Authentication Agent Fallback

### Trigger

Invalid phone number
Invalid address
Missing information
Authentication failure

### Prompt

I couldn't verify your information.

Please provide your registered phone number and service address again.

This information is required to securely access your account.
After 3 failures
I'm unable to verify your identity after multiple attempts.

I'll transfer you to a customer support representative for further assistance.

## 3. Service Status Agent Fallback

### Trigger

Customer asks unrelated question.

### Example

**Customer:**
Tell me today's weather.

**Bot:**
I'm currently helping you with your waste collection schedule.

If you need help with another service, I'll return you to the main assistant.
Transfer back to Root Agent.

## 4. Missed Pickup Agent Fallback

### Trigger

Missing information.

**Bot:**
Could you tell me which bin wasn't collected?

For example:
• General Waste
• Recycling
• Organic Waste
If date missing
Which pickup date are you referring to?

## 5. Reschedule Agent Fallback

### Trigger

Invalid date
Past date
Ambiguous date

**Bot:**
Please provide a valid future pickup date.

For example:
Tomorrow
Friday
15 July

## 6. Route Delay Agent Fallback

### Trigger

User doesn't specify pickup.

**Bot:**
Are you asking about today's waste collection delay?

Please let me know your pickup date if it's different.

## 7. Container Blockage Agent Fallback

### Trigger

Unknown blockage.

**Bot:**
Could you describe what prevented the collection?

Examples:
• Parked vehicle
• Road blockage
• Overflowing container

## 8. Complaint Agent Fallback

### Trigger

Complaint too vague.

**Customer:**
Complaint.

**Bot:**
Could you briefly describe your complaint so I can register it correctly?

## 9. Escalation Agent Fallback

### Trigger

Even escalation fails.

**Bot:**
I'm unable to connect you with a representative at the moment.

Please try again later or contact our customer support center directly.

## 10. Tool Failure Fallback

### Trigger

Every OpenAPI Tool
Timeout
500 Error
Invalid Response
Connection Failure

### Prompt

I'm sorry, but I'm unable to access our service system right now.

Please try again in a few minutes.

If the issue continues, I'll connect you with customer support.

## 11. No Match Fallback

First Time
I didn't quite understand that.

Could you rephrase your request?
Second Time
I'm still not sure I understood.

You can ask me things like:

• Check my pickup schedule

• Report a missed pickup

• Reschedule a pickup

• Register a complaint
Third Time
I'm unable to understand your request.

I'll connect you with a customer support representative.

## 12. No Input Fallback

First Silence
I didn't receive your response.

Could you please repeat that?
Second Silence
I'm still waiting for your response.

Please let me know how I can assist you.
Third Silence
Since I haven't received a response, I'll end this conversation.

Thank you for contacting GreenCycle Waste Services.

## 13. Human Request Fallback

**Customer:**
I want a human.

**Bot:**
Certainly.

I'll transfer you to one of our customer support representatives.

## 14. Unsupported Request Fallback

**Customer:**
Book my flight.

**Bot:**
I'm here to assist with GreenCycle Waste Services.

I can help with:

• Pickup schedules

• Missed pickups

• Route delays

• Complaints

• Rescheduling

## 15. Session Timeout Fallback

For your security, your session has expired.

Please verify your account again before we continue.

## 16. Authentication Required Fallback

**Customer:**
Register my complaint.

**Session:**
authenticated = false

**Bot:**
Before I can help with that, I'll need to verify your account.

Please provide your registered phone number.

## 17. Confirmation Fallback

**Customer:**
Maybe.

**Bot:**
I didn't quite understand your response.

Would you like me to proceed?

Please reply with Yes or No.

## 18. Business Rule Fallback

**Customer:**
Reschedule yesterday's pickup.

**Bot:**
Pickup requests can only be rescheduled to a future available date.

Please choose another date.

## 19. Duplicate Request Fallback

**Bot:**
It looks like a similar request has already been submitted.

Would you like to check its current status instead?

## 20. Global Fallback (Recommended)

Attach this at the agent level so every sub-agent follows the same behavior:
If you cannot understand the customer's request:

1. Politely apologize.
2. Ask the customer to rephrase the request.
3. Provide examples of supported services.
4. Never guess the customer's intent.
5. If the request remains unclear after two attempts, transfer the conversation to the Escalation Agent.

If an external tool fails:

1. Inform the customer that the system is temporarily unavailable.
2. Do not expose technical details or error codes.
3. Ask the customer to try again later.
4. If the issue persists, transfer the conversation to the Escalation Agent.

If the customer requests a human representative, transfer them immediately.

If the customer becomes inactive, prompt twice before ending the conversation.

## Recommended fallback structure for your project

This gives you a complete enterprise fallback strategy that fits your Waste Management Assistant architecture and is suitable for implementation in CX Agent Studio.

## 21. Invalid Date Format Fallback

### Trigger

Customer enters an invalid date.

**Customer:**
32 July

**Bot:**
I couldn't recognize that date.

Please provide a valid future date.

For example:
• Tomorrow
• Friday
• 15 July 2026

## 22. Past Date Fallback

**Customer:**
Move it to yesterday.

**Bot:**
Pickup requests can't be rescheduled to a past date.

Please choose a future available date.

## 23. Unsupported Bin Type

**Customer:**
Purple bin

**Bot:**
I couldn't identify that bin type.

Please choose one of the following:

• General Waste
• Recycling
• Organic Waste

## 24. Missing Confirmation

**Bot:**
Would you like me to submit this request?

**Customer:**
Maybe

**Bot:**
I didn't quite understand.

Please reply with:

• Yes
• No

## 25. Customer Interrupts Authentication

**Customer:**
Why do you need my phone number?

**Bot:**
Your phone number helps us verify your account securely before accessing your service information.

Once verified, I'll continue with your request.

## 26. User Changes Intent Mid-Conversation

**Customer:**
Report a missed pickup.
Bot asks authentication.

**Customer:**
Actually I want my pickup schedule.

**Bot:**
No problem.

We'll complete your verification first, then I'll help you check your pickup schedule.

## 27. Customer Says "I Don't Know"

**Bot:**
Which pickup date?

**Customer:**
I don't know.

**Bot:**
That's okay.

Could you tell me approximately when the pickup was scheduled?

For example:

• Today
• Yesterday
• Last Friday

## 28. Duplicate Reschedule Request

Tool detects pickup already rescheduled.

**Bot:**
Your pickup has already been rescheduled.

Would you like me to tell you the updated pickup date?

## 29. Customer Wants Status of Existing Ticket

**Customer:**
I already reported this.

**Bot:**
Would you like me to check the status of your existing service request?

## 30. Unsupported Service

**Customer:**
I want to pay my electricity bill.

**Bot:**
I'm here to help with GreenCycle Waste Services.

I can assist with:

• Pickup schedules

• Missed pickups

• Complaints

• Route delays

• Pickup rescheduling

## 31. Multiple Requests Without Authentication

**Customer:**
Check my pickup, report a complaint and reschedule.

**Bot:**
I'd be happy to help.

Before we continue, I'll need to verify your account.

Please provide your registered phone number.

## 32. Session Variables Missing

**Session:**
authenticated = true
customer_id = null

**Bot:**
Your session information appears to be incomplete.

For security, let's verify your account again.

## 33. Customer Gives Phone Number in Words

**Customer:**
Nine eight seven six...

**Bot:**
Thank you.

Let me verify your registered phone number.

## 34. Customer Uses Short Answers

**Bot:**
Describe your complaint.

**Customer:**
Bad.

**Bot:**
Could you provide a little more detail so I can register your complaint accurately?

## 35. Tool Returns Empty JSON

**Tool:**
{}

**Bot:**
I couldn't retrieve your information at the moment.

Please try again shortly.

## 36. API Returns Invalid Response

**Bot:**
Our service system returned an unexpected response.

Please try again later.

## 37. Authentication Tool Timeout

**Bot:**
I'm having trouble verifying your account right now.

Please try again in a few minutes.

## 38. Customer Asks "Why?"

Bot asks confirmation.

**Customer:**
Why?

**Bot:**
I ask for confirmation to make sure I don't submit any request without your approval.

## 39. Customer Refuses Confirmation

**Customer:**
I don't want to confirm.

**Bot:**
That's okay.

I won't submit the request without your confirmation.

Let me know if you'd like to continue later.

## 40. User Requests Human During Authentication

**Customer:**
I want a person.

**Bot:**
Certainly.

I'll connect you with a customer support representative.

## 41. Pickup Already Completed

**Customer:**
Cancel today's pickup.

**Bot:**
Today's pickup has already been completed and can't be cancelled.

## 42. Pickup Already Missed

**Customer:**
Reschedule yesterday's pickup.

**Bot:**
Since the scheduled pickup has already passed, it can't be rescheduled.

Would you like to report it as a missed pickup instead?

## 43. Complaint Already Closed

**Bot:**
This complaint has already been resolved.

Would you like to register a new complaint instead?

## 44. Customer Provides Extra Information

**Customer:**
My phone is 9876543210 and my Aadhaar is...

**Bot:**
Thank you.

I'll use your registered phone number for verification.

No additional personal information is required.

## 45. Customer Tries SQL Injection/Junk Input

**Customer:**
DROP TABLE users;

**Bot:**
I couldn't understand your request.

Please tell me how I can help with your waste management service.

## 46. Customer Sends Emoji Only

**Customer:**
😊😊😊

**Bot:**
I'm here to help with your waste management services.

How may I assist you today?

## 47. Customer Uses Only Numbers

**Customer:**
123456

**Bot:**
Could you please tell me how I can help?

For example:
• Check pickup status
• Report missed pickup
• Register complaint

## 48. Customer Says "Continue"

Bot lost context.

**Customer:**
Continue.

**Bot:**
Could you tell me which request you'd like to continue?

For example:

• Pickup schedule

• Complaint

• Missed pickup

## 49. User Says "Start Over"

**Bot:**
Certainly.

Let's start again.

How may I help you with your waste management service today?
(Session can optionally be reset if appropriate.)

## 50. Unexpected Internal Error

Catch-all fallback.
I apologize, but something unexpected happened while processing your request.

Please try again later.

If the issue continues, I'll connect you with customer support.

## Enterprise-Level Fallback Categories

For your project, organize fallbacks into these categories:
