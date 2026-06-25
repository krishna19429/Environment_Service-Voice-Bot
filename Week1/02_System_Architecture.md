### Overview
The system uses a multi-agent architecture with a single Root Agent acting as orchestrator and 7 specialized Sub-Agents each owning one service domain. All agents share session state via CX session parameters. Backend operations are performed via Cloud Function webhooks. Voice is handled by Google Phone Gateway.

### Architecture Layers 

|Layer| Component| Technology| Responsibility|
|-----|----------|-----------|---------------|
|1. Voice| Phone Gateway| Google Phone Gateway| Receive call, STT (speech→text), TTS (text→speech), barge-in|
|2.Orchestration| Root Agent| CX Agent Studio Playbook| Greet, authenticate, classify intent, route to Sub-Agent|
|3.Authentication| Authentication Agent| CX Agent Studio Sub-Agent| Collect phone + address, call validateAddress(), set session params|
|4. Service| 7 Sub-Agents| CX Agent Studio Sub-Agent Playbooks| Handle specific service journey end-to-end|
|5. Tools| 9 Python Tools| CX Agent Studio Tool definitions| LLM-callable wrappers for each API function|
|6. Webhook Cloud Function| Python 3.11 on GCP| Single HTTPS endpoint routing all API calls via tag|
|7. Data| Mock Database| Python dict in Cloud Function |Customer records, service data (Week 1)|

### Business Objective - Data Flow
The following describes a complete call from ring to resolution:

#### COMPLETE CALL DATA FLOW 
- Step 01: Customer calls the Phone Gateway number 
- Step 02: Phone Gateway receives call, converts speech to text (STT) 
- Step 03: Text sent to Root Agent Playbook 
- Step 04: Root Agent greets caller, captures initial intent 
- Step 05: Root Agent routes to Authentication Agent 
- Step 06: Authentication Agent collects phone number (@sys.phone-number entity) 
- Step 07: Authentication Agent collects service address (@sys.any entity) 
- Step 08: Authentication Agent calls validateAddress webhook (Cloud Function tag: validate_address) 
- Step 09: Cloud Function queries mock database, returns {verified: true, customer_id: C001}
- Step 10: Session params set: is_authenticated=true, customer_id=C001
- Step 11: Root Agent receives control, routes to correct Sub-Agent based on pre-classified intent
- Step 12: Sub-Agent executes service logic (e.g., MissedPickupTool calls reportMissedPickup())
- Step 13: Cloud Function creates ticket, returns {ticket_id: MP-C001-20260620, resolution_date: 2026-06-21}
- Step 14: Sub-Agent reads response, formulates confirmation message
- Step 15: Response sent back to CX, then to Phone Gateway as TTS
- Step 16: Customer hears confirmation, call ends or continues to next query

### Technical Implementation — Agent Routing Matrix

|From Agent| Condition |To Agent |Data Passed|
|----------|-----------|---------|-----------|
|Root Agent| Any service intent detected |Authentication Agent| Pre-classified intent|
|Authentication Agent| verified=true |Root Agent| customer_id, is_authenticated=true|
|Authentication Agent| verified=false after 3 tries| Escalation Agent| fail_reason, attempt_count|
|Root Agent |intent=missed_pickup |Missed Pickup Agent |customer_id, is_authenticated|
|Root Agent |intent=reschedule |Reschedule Agent |customer_id, is_authenticated|
|Root Agent |intent=check_status |Service Status Agent |customer_id, is_authenticated|
|Root Agent |intent=route_delay |Route Delay Agent |customer_id, is_authenticated|
|Root Agent |intent=blockage |Container Blockage Agent| customer_id, is_authenticated|
|Root Agent |intent=complaint |Complaint Agent |customer_id, is_authenticated|
|Any Sub-Agent| API failure after retry |Escalation Agent |Full conversation summary|
|Any Sub-Agent |Customer requests human |Escalation Agent |Full conversation summary|
|Any Sub-Agent |3 no-match strikes |Escalation Agent| Full conversation summary|

### Session Parameters — Complete Reference
|Parameter| Type| Set By| Read By| Example Value|
|---------|-----|-------|--------|--------------|
|is_authenticated| Boolean| Authentication Agent| All Sub-Agents (gate)| true|
|customer_id |String |Authentication Agent| All Sub-Agent tools| C001|
|customer_name |String| Authentication Agent| All agents (personalization)| Ankit Sharma|
|user_phone |String| Authentication Agent |Root Agent, Escalation |9876543210|
|user_address| String| Authentication Agent |Rescue, Missed Pickup| Baner, Pune|
|current_intent |String| Root Agent |Root Agent (re-routing) |service.missed_pickup|
|last_ticket_id |String| Active Sub-Agent| Escalation Agent| MP-C001-20260620|
|auth_fail_count |Integer| Authentication Agent |Auth Agent (3-strike) |0|
|complaint_desc |String| Complaint Agent| ComplaintTool |Free text|
|new_date| Date| Reschedule Agent| RescheduleTool| 2026-06-23|
  
