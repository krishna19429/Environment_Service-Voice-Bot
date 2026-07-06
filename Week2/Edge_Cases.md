## This document outlines edge cases for our voice agent.

### 1. Invalid Address
#### Scenario :
The user provide an address that does not exist, cannot be validated, or is outside the service area.

#### Agent Response:
"We were unable to verify the address provided. Please confirm your service address."

#### Resolution:

|Condition | Action |
|----------|--------|
Address Valid | Continue Registration|
|Address Invalid|  Request Address Again|
|3 Failed Attempts | Transfer to Human Agent|

### 2. Authentication Failure
#### Scenario:
Customer authentication fails due to incorrect credentials.

#### Agent Response:
"I could not verify your account. Please try again."

#### Resolution:
|Condition|Action|
|---------|------|
Authentication Success| Continue Flow|
|Authenticate Failure | Retry|
|3 Consecutive Failures| Escalate to Human Agent|

### 3. Missing Service Date
#### Scenario
Customer does not provide a service date.

#### Agent Response 
"Could you please provide the pickup service date you are referring to?"

#### Resolution
|Condition	|Action|
|---------|-----------|
|Date Provided|	Validate Date|
|Date Missing|	Reprompt|
|Multiple Failed Attempts|Human Agent|

#### Route Delay API Failure
#### Scenario:
The route management service is unavailable.


#### Agent Response
"I am currently unable to retrieve route information. Please try again shortly."

#### Resolution
|Condition|	Action|
|---------|-------|
|Retry Successful	|Continue|
|Retry Failed|	Inform User|
|Repeated Failure	|Human Agent|

### 4. Invalid Reschedule Requests
#### No Available Pickup Slots
##### Scenario:
All pickup slots for the request date are fully booked.
#### Action : Suggest next available date.


