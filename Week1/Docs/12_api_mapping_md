# 12 — API Mapping 

 

## Webhook Request Format (CX → Cloud Function) 

 

```json 

{ 

  "fulfillmentInfo": { 

    "tag": "report_missed_pickup" 

  }, 

  "sessionInfo": { 

    "parameters": { 

      "customer_id": "C001", 

      "missed_date": "2026-06-20", 

      "is_authenticated": true 

    } 

  } 

} 

``` 

 

## Webhook Response Format (Cloud Function → CX) 

 

```json 

{ 

  "fulfillmentResponse": { 

    "messages": [ 

      {"text": {"text": ["Ticket MP-C001-20260620 created. Resolution: 2026-06-21"]}} 

    ] 

  }, 

  "sessionInfo": { 

    "parameters": { 

      "ticket_id": "MP-C001-20260620", 

      "resolution_date": "2026-06-21" 

    } 

  } 

} 

``` 

 

--- 

 

## API Mapping Table 

 

| CX Tool | Webhook Tag | Python Function | Input | Output | 
|---------|-------------|-----------------|-------|--------| 
| AddressValidationTool | validate_address | validate_address(params) | phone, address | verified, customer_id | 
| ServiceStatusTool | check_service_status | check_service_status(params) | customer_id | status, date | 
| MissedPickupTool | report_missed_pickup | report_missed_pickup(params) | customer_id, date | ticket_id, resolution | 
| RescheduleTool | reschedule_pickup | reschedule_pickup(params) | customer_id, new_date | new_date, confirmation_id | 
| RouteDelayTool | get_route_delay | get_route_delay(params) | customer_id | delay, reason, eta | 
| ContainerCheckTool | check_container | check_container(params) | customer_id | blocked, ticket_id | 
| ServiceRequestTool | create_complaint | create_complaint(params) | customer_id, desc | ticket_id, follow_up | 
| CustomerInfoTool | get_customer_info | get_customer_info(params) | customer_id | name, address, date | 
| NextPickupTool | get_next_pickup | get_next_pickup(params) | customer_id | next_date, slot | 

 

--- 

 

## Retry Logic 

 

- All non-complaint webhooks: retry once after 2-second delay on timeout 

- Complaint webhook: no retry — escalate immediately on failure 

- Auth webhook: no retry — prompt caller to try again (up to 3 human attempts) 

- CX webhook timeout: set to 5 seconds in Manage > Webhooks settings 

- Cloud Function timeout: 10 seconds
