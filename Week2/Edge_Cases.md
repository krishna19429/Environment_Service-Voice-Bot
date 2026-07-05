### Overview 
Edge cases are all the scenarios that deviate from the happy path. Handling these correctly is the difference between a 70% containment rate and a 90% containment rate. Week 2 of the sprint focuses primarily on implementing and testing these scenarios. 

### Authentication Failures
|Scenario |Trigger |Bot Behaviour |System Action |
|---------|--------|--------------|--------------|
|Wrong address |Address does not match phone in DB |Inform caller: 'The address does not match. Please try again with your full service address including area and city.' |System ActionIncrement auth_fail_count. Re-collect address |
|Phone not found |Phone number not in mock DB |Inform caller: 'I could not find an account with that number. Please check and try again.' |Increment auth_fail_count. Re-collect phone + address |
|Third failure |auth_fail_count reaches 3 |Inform: 'I was unable to verify your account. Let me connect you with an agent.' |Transfer to Escalation Agent. Session summary passed. |
|Caller hangs up during auth |No input during phone/address collection |Three-strike no-input handler fires: 'I have not heard a response. Thank you for calling. Goodbye.' |End session gracefully |


### API Timeout and Failure 
|Scenario |Agent Affected  |Bot Behaviour |System Action |
|---------|--------|--------------|--------------|
|validateAddress times out |Authentication Agent |'I am experiencing a technical issue. Please hold while I connect you.'  |Escalate immediately — cannot authenticate without API  |
|reportMissedPickup fails |Missed Pickup Agent  |'I am sorry, I was unable to log your complaint right now. Let me connect you with an agent who can help.'  |Retry once, then escalate with reason=api_failure  |
|reschedulePickup fails |Reschedule Agent |'I was unable to update your schedule. Let me connect you with our team.'  |Escalate — do not auto-retry reschedule (risk of double booking)  |
|createServiceRequest fails (complaint) |Complaint Agent  |'I sincerely apologize. Let me connect you immediately with a senior agent.'  |Escalate immediately — never retry on frustrated caller |


### No-Match and No-Input
|Strike |No-Input Response  |No-Match Response |
|---------|--------|--------------|
|Strike 1  |'I did not hear anything. Could you please repeat that?' |'I did not quite catch that. I can help with missed pickups, rescheduling, service status, route delays, container issues, and complaints.'  |
|Strike 2 |'Are you still there? Please say what you need help with — for example: missed pickup, or service status.'  |'Could you say one of the following: missed pickup, reschedule, check status, or complaint?' |
|Strike 3 |'I have not heard a response. Thank you for calling Waste Management Services. Goodbye.' |'I am having difficulty understanding your request. Would you like me to connect you with a live agent?'|

### ### Overview 
Edge cases are all the scenarios that deviate from the happy path. Handling these correctly is the difference between a 70% containment rate and a 90% containment rate. Week 2 of the sprint focuses primarily on implementing and testing these scenarios. 

### Authentication Failures
|Scenario |Trigger |Bot Behaviour |System Action |
|---------|--------|--------------|--------------|
|Wrong address |Address does not match phone in DB |Inform caller: 'The address does not match. Please try again with your full service address including area and city.' |System ActionIncrement auth_fail_count. Re-collect address |
|Phone not found |Phone number not in mock DB |Inform caller: 'I could not find an account with that number. Please check and try again.' |Increment auth_fail_count. Re-collect phone + address |
|Third failure |auth_fail_count reaches 3 |Inform: 'I was unable to verify your account. Let me connect you with an agent.' |Transfer to Escalation Agent. Session summary passed. |
|Caller hangs up during auth |No input during phone/address collection |Three-strike no-input handler fires: 'I have not heard a response. Thank you for calling. Goodbye.' |End session gracefully |


### API Timeout and Failure 
|Scenario |Agent Affected  |Bot Behaviour |System Action |
|---------|--------|--------------|--------------|
|validateAddress times out |Authentication Agent |'I am experiencing a technical issue. Please hold while I connect you.'  |Escalate immediately — cannot authenticate without API  |
|reportMissedPickup fails |Missed Pickup Agent  |'I am sorry, I was unable to log your complaint right now. Let me connect you with an agent who can help.'  |Retry once, then escalate with reason=api_failure  |
|reschedulePickup fails |Reschedule Agent |'I was unable to update your schedule. Let me connect you with our team.'  |Escalate — do not auto-retry reschedule (risk of double booking)  |
|createServiceRequest fails (complaint) |Complaint Agent  |'I sincerely apologize. Let me connect you immediately with a senior agent.'  |Escalate immediately — never retry on frustrated caller |


### No-Match and No-Input
|Strike |No-Input Response  |No-Match Response |
|---------|--------|--------------|
|Strike 1  |'I did not hear anything. Could you please repeat that?' |'I did not quite catch that. I can help with missed pickups, rescheduling, service status, route delays, container issues, and complaints.'  |
|Strike 2 |'Are you still there? Please say what you need help with — for example: missed pickup, or service status.'  |'Could you say one of the following: missed pickup, reschedule, check status, or complaint?' |
|Strike 3 |'I have not heard a response. Thank you for calling Waste Management Services. Goodbye.' |'I am having difficulty understanding your request. Would you like me to connect you with a live agent?'|

### Overview 
Edge cases are all the scenarios that deviate from the happy path. Handling these correctly is the difference between a 70% containment rate and a 90% containment rate. Week 2 of the sprint focuses primarily on implementing and testing these scenarios. 

### Authentication Failures
|Scenario |Trigger |Bot Behaviour |System Action |
|---------|--------|--------------|--------------|
|Wrong address |Address does not match phone in DB |Inform caller: 'The address does not match. Please try again with your full service address including area and city.' |System ActionIncrement auth_fail_count. Re-collect address |
|Phone not found |Phone number not in mock DB |Inform caller: 'I could not find an account with that number. Please check and try again.' |Increment auth_fail_count. Re-collect phone + address |
|Third failure |auth_fail_count reaches 3 |Inform: 'I was unable to verify your account. Let me connect you with an agent.' |Transfer to Escalation Agent. Session summary passed. |
|Caller hangs up during auth |No input during phone/address collection |Three-strike no-input handler fires: 'I have not heard a response. Thank you for calling. Goodbye.' |End session gracefully |


### API Timeout and Failure 
|Scenario |Agent Affected  |Bot Behaviour |System Action |
|---------|--------|--------------|--------------|
|validateAddress times out |Authentication Agent |'I am experiencing a technical issue. Please hold while I connect you.'  |Escalate immediately — cannot authenticate without API  |
|reportMissedPickup fails |Missed Pickup Agent  |'I am sorry, I was unable to log your complaint right now. Let me connect you with an agent who can help.'  |Retry once, then escalate with reason=api_failure  |
|reschedulePickup fails |Reschedule Agent |'I was unable to update your schedule. Let me connect you with our team.'  |Escalate — do not auto-retry reschedule (risk of double booking)  |
|createServiceRequest fails (complaint) |Complaint Agent  |'I sincerely apologize. Let me connect you immediately with a senior agent.'  |Escalate immediately — never retry on frustrated caller |


### No-Match and No-Input
|Strike |No-Input Response  |No-Match Response |
|---------|--------|--------------|
|Strike 1  |'I did not hear anything. Could you please repeat that?' |'I did not quite catch that. I can help with missed pickups, rescheduling, service status, route delays, container issues, and complaints.'  |
|Strike 2 |'Are you still there? Please say what you need help with — for example: missed pickup, or service status.'  |'Could you say one of the following: missed pickup, reschedule, check status, or complaint?' |
|Strike 3 |'I have not heard a response. Thank you for calling Waste Management Services. Goodbye.' |'I am having difficulty understanding your request. Would you like me to connect you with a live agent?'|


### Invalid Date in Reschedule
* Caller says a date in the past: 'Reschedule to last Monday' 

* Bot response: 'I am sorry, I can only reschedule to a future date. Could you please provide a date that is today or later?' 

* Caller says an ambiguous date: 'Next week sometime' 

* Bot response: 'Which day next week would you prefer? For example, Monday, Tuesday, or Wednesday?' 
* Bot response: 'The date you selected falls on a public holiday when we do not operate. The next available date is [date]. Shall I reschedule to that date instead?'


### Holiday Scheduling Edge Case 

* Caller requests reschedule to a national holiday 

* Bot checks holiday calendar, informs caller, offers next available weekday 

* System auto-suggests next available date


### Out-of-Scope Requests 

* Caller asks about billing, payments, account creation, contract changes 

* Bot response: 'I can only assist with waste management service queries such as missed pickups, rescheduling, status checks, and complaints. For account-related matters, let me connect you with our customer care team.' 

* Route to Escalation Agent with reason=out_of_scope 
