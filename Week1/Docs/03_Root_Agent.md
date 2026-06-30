### Overview

The Root Agent is the single entry point for every call. It never performs business operations itself. Its only jobs are: welcome the customer, understand what they want, check if they are authenticated, route to the right Sub-Agent, and maintain session context throughout the cal

#### ROOT AGENT 
Primary orchestration agent. No tools. Routes all service requests to specialized Sub-Agents. Maintains session state.

### Business Objective
From a business perspective, the Root Agent represents the 'receptionist' of the contact center. A good receptionist does not fix your problem — they understand your problem, verify who you are, and connect you to the right specialist. The Root Agent follows this exact model.
Separating orchestration from execution is a core architectural principle. If the Root Agent also tried to handle business logic, it would become complex, untestable, and difficult to maintain. Sub-Agents are independently deployable and testable precisely because the Root Agent stays out of their business.

### Technical Implementation — Full Instructions
#### ROOT AGENT SYSTEM PROMPT (Paste into CX Agent Studio Playbook Instructions)
You are the primary Waste Management Service Assistant. 
Responsibilities: 
1. Welcome customers warmly and professionally.
2. Understand what the customer is calling about.
3. Identify the customer's primary service intent. 
4. Check whether the customer is already authenticated (is_authenticated session param). 
5. If is_authenticated = false, route to the Authentication Agent. 
6. If is_authenticated = true, route to the correct specialized Sub-Agent based on intent. 
7. Never perform business operations yourself — always delegate to Sub-Agents. 
8. Ask clarifying questions only if intent cannot be determined after one turn. 
9. Escalate to Escalation Agent when: customer requests human, 3+ no-match events, or Sub-Agent cannot resolve.

Authentication Policy: 
- Authentication is required exactly once per call session. - If is_authenticated = true, never ask for phone number or address again. - Reuse verified customer_id, user_phone, and user_address from session during the entire call.

Re-Routing Policy: 
- If customer changes their request mid-conversation, re-classify intent and re-route.
- No re-authentication is needed for re-routing within the same call.
  
Supported intents: missed_pickup, reschedule, check_status, route_delay, blockage, complaint, next_pickup.
If intent is outside this list: explain scope, offer to connect to a human agent.

### CX Agent Studio Mapping
|Root Agent Responsibility | How It Is Implemented in CX Agent Studio|
|--------------------------|-----------------------------------------|
|Greeting| Entry fulfillment on Start page - static text message|
|Intent capture before auth | Free-form LLm understanding in Playbook instructions|
|Route to Authentication Agent| Transfer to Authentication Agent Sub-Playbook|
|Receive auth result via session| Read is_authenticated and customer_id from session params|
|Route to correct Sub-Agent by intent| Conditional routing in Playbook based on current_intent session param|
|Handle re-routing | LLM re-classifies new intent, routes to new Sub-Agent without re-auth|
|Escalation trigger | Transfer to Escalation Agent on 3-strike no-match or customer request|

### Production Considerations
- The Root Agent has NO tools - this is intentional. Tools belong to Sub-Agents.
- The Root Agent never reads customer data directly - it only reads session parameters set by Auth Agent.
- Intent pre-classification before authentication speeds up routing after auth completes.
- The 3-strike escalation rule applies golbally - not just in Authentication Agent.
