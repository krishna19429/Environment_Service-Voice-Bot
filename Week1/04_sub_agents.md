### Overview 
There are 8 Sub-Agents in total. One Authentication Agent handles identity verification. Seven Service Sub-Agents each handle one specific service journey end-to-end. Every Sub-Agent verifies authentication status before executing any business operation.

### Authentication Agent
|Verifies customer identify using registered phone number and service address. The only agent that interacts with identify data.|
|:-------------------------------------------------------------------------------------------------------------------------------|

### Inputs 
- Registered phone number (collected via @sys.phone-number entity)
- Service address (collected via @sys.any.entity)

### Process
- Collect phone number — prompt: 'Please say your 10-digit registered phone number'
- Collect service address — prompt: 'Please confirm your service address'
- Call AddressValidationTool → validateAddress(phone, address)
- On success: set is_authenticated=true, customer_id, user_phone, user_address in session — transfer back to Root Agent
- On failure: inform caller, offer retry — allow up to 3 attempts
- After 3 failures: transfer to Escalation Agent with reason='auth_failed'

### Outputs
- Session: is_authenticated=true/false
- Session:customer_id(C001-C004)
- Session: user_phone, user_address

### Service Status Agent
|Provides service status, pickup schedules, and upcoming collection information. Read-only — no write operations.|
|----------------------------------------------------------------------------------------------------------------|

### Inputs
- customer_id (from session - set by the Auth Agent)

### Process 
- Verify is_authenticated=true - if false, transfer to Authentication Agent
- Call ServiceStatusTool => checkServiceStatus(customer_id)
- Read back: status, service date, location
- If status=Delayed, proactively offer to fetch route delay details

## MISSED PICKUP AGENT
|Handles missed garbage collection reports. Creates service tickets and provides resolution timelines.|
|-----------------------------------------------------------------------------------------------------|

### Inputs

- customer_id (session)
- missed_date (@sys.date — collected if not already known)

### Process

- Verify is_authenticated=true
- Confirm missed date — if caller says 'today' resolve from system date and confirm back
- Call MissedPickupTool → reportMissedPickup(customer_id, missed_date)
- Read back ticket ID and resolution date to caller

### Outputs

- ticket_id (e.g., MP-C001-20260620)
- resolution_date (next business day)

## RESCHEDULE AGENT
|Handles pickup rescheduling requests. Requires explicit confirmation before executing schedule changes.|
|-------------------------------------------------------------------------------------------------------|

### Inputs

- customer_id (session)
- new_date (@sys.date — collected from caller)

#Process

- Verify is_authenticated=true
- Ask caller for desired new date
- Extract new_date using @sys.date entity
- CONFIRM: 'You said [date]. Is that correct?' — WAIT for yes before calling API
- On confirmation: call RescheduleTool → reschedulePickup(customer_id, new_date)
- Read back new date and confirmation ID


### Key Rule
- NEVER call reschedulePickup() without explicit verbal confirmation from caller

# ROUTE DELAY AGENT
|Provides real-time route delay information and estimated arrival times. Read-only.|
|----------------------------------------------------------------------------------|

### Inputs
- customer_id (session) — used to find route_id

### Process
- Verify is_authenticated=true
- Call RouteDelayTool → getRouteDelayInfo(route_id) using customer's route
- If delay exists: state hours, reason, ETA
- If no delay: confirm route is on schedule
- If delay > 4 hours: proactively offer reschedule or complaint

## CONTAINER BLOCKAGE AGENT
|Handles inaccessible or blocked container reports. Creates priority service tickets.|
|------------------------------------------------------------------------------------|

### Inputs
- customer_id (session)
- blockage_description (free text — collected from caller)

### Process
- Verify is_authenticated=true
- Ask caller to briefly describe the blockage
- Call ContainerCheckTool → checkContainerStatus(customer_id)
- Create priority ticket, read back ticket ID
- Follow-up ETA: 'A team will attend within 2 hours'
- If blockage is persistent: offer to trigger reschedule via Reschedule Agent

### COMPLAINT AGENT 
|Handles formal complaints with empathy. Commits to 24-hour callback. Designed for frustrated callers.|
|-----------------------------------------------------------------------------------------------------|

### Instructions for Emotional Handling
- FIRST: Acknowledge frustration — 'I sincerely apologize for the inconvenience'
- NEVER be defensive — do not justify or explain the service failure
- THEN: Collect complaint description via @sys.any
- Call ServiceRequestTool → createServiceRequest(customer_id, description)
- Read back formal ticket ID
- Commit: 'A senior representative will call you within 24 hours'
- If caller remains very upset after ticket logged: transfer to Escalation Agent immediately
- On API failure: DO NOT ask frustrated caller to retry — escalate immediately

### ESCALATION AGENT 
|Transfers unresolved conversations to human agents. No tools. Passes full session context.|
|------------------------------------------------------------------------------------------|

### Triggers
- Authentication fails 3 consecutive times
- Any API unavailable after retry
- Customer explicitly requests human agent
- Customer appears significantly distressed
- 3+ consecutive no-match events on any agent

### Process
- Acknowledge the transfer warmly: 'Let me connect you to one of our specialists who can help you right away'
- Compile full session summary: customer_id, intents tried, tickets created, reason for escalation
- Transfer conversation context to live agent queue
- Estimated wait time if available: 'Estimated wait is approximately 3 minutes'
