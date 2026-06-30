### Overview
There are 7 primary service intents. Each intent maps to exactly one Sub-Agent. NLU target accuracy is above 85% on first attempt. A minimum of 15 training phrases per intent is required. Intents are named using a service prefix to avoid naming conflicts.

|Intent Name| Frequency| Sub-Agent| Training Phrases| NLU Target|
|-----------|----------|----------|-----------------|-----------|
|service.missed_pickup| ~35% of calls| Missed Pickup Agent| 20+ phrases| > 90%|
|service.reschedule| ~25% of calls| Reschedule Agent| 18+ phrases| > 88%|
|service.check_status| ~15% of calls| Service Status Agent| 15+ phrases| > 85%|
|service.route_delay| ~10% of calls| Route Delay Agent| 15+ phrases| > 85%|
|service.blockage| ~8% of calls| Container Blockage Agent| 15+ phrases| > 85%|
|service.complaint| ~5% of calls| Complaint Agent| 15+ phrases| > 85%|
|service.next_pickup| ~2% of calls| Service Status Agent| 12+ phrases| > 85%|

### Intent: service.missed_pickup
#### Training Phrases
- My garbage was not picked up
- Pickup was missed today
- Nobody came to collect the waste
- The truck skipped my house
- My bin was not emptied
- Garbage collection did not happen
- Driver did not come
- Collection was missed
- My trash was not taken
- Waste was not collected
- Service was not completed today
- No pickup this morning
- Garbage truck forgot my house
- My container was not serviced
- Waste collection did not happen
- Pickup was not done
- Nobody came today
- My garbage is still here
- The van did not come
- They missed my stop

### Intent: service.reschedule
#### Training Phrases
- Reschedule my pickup
- Change my collection date
- I want a different pickup day
- Move my garbage collection
- Can you shift my pickup
- I need to change my service date
- Reschedule waste collection
- Can I change when you come
- Move my pickup to next week
- Change my garbage day
- Postpone my pickup
- Push my collection to Thursday
- Change my pickup schedule
- Shift my service date
- I want a new pickup date
- Can we reschedule
- Move it to next Monday
- I need to delay my collection

### Intent: service.check_status
#### Training Phrases
- Check my service status
- What is my pickup status
- Is my collection confirmed
- Tell me my service details
- Service status please
- What day is my next garbage collection
- When is my next pickup
- Is my collection scheduled
- When will you collect my garbage
- Tell me about my pickup
- What is scheduled for me
- Is my service date confirmed
- Check pickup status
- Do you have me scheduled
- What is my service status today

### Intent: service.route_delay
#### Training Phrases
- Why is the truck late
- Is there a delay on my route
- Garbage truck has not come yet
- What is the ETA
- Is collection running late
- When will the truck come
- Is my route delayed
- How long is the delay
- Will the truck come today
-  What time will you get here
-  Is there a problem with my service
-  Expected arrival time of garbage truck
-  Route delay information
-  How many hours delayed
-  Any delay in my area

### Intent: service.blockage
#### Training Phrases
- My container is blocked
- Bin not accessible
- Truck cannot reach my garbage
- Something is blocking the dumpster
- Container access problem
- There is a car near my bin
- The gate is locked and truck cannot enter
- Container is not accessible to the truck
- Blockage near my container
- My bin is blocked off
- Vehicle is blocking my container
- Construction near my garbage bin
- I have an obstruction issue
- My bin is not accessible today
- The road to my bin is blocked

### Intent: service.complaint
#### Training Phrases
- I want to raise a complaint
- Register my grievance
- File a formal complaint
- I am very unhappy with the service
- This is completely unacceptable
- I want to escalate this issue
- Complaint about waste collection
- I am frustrated with your service
- This service is terrible
- I need to file a formal complaint
- I am extremely upset
- This is the third time this has happened
- I need to speak to a manager
- Your service is very poor
- I am not happy with the service at all

### Intent: service.next_pickup
#### Training Phrases
- When is my next pickup
- Next collection date
- Tell me my upcoming pickup date
- When is garbage collection next
- What day is my next service
- Upcoming pickup schedule
- When will you come next
- What is my next collection date
- When will you collect next
- Next service date please
- When is my next scheduled service
- When is my service next
