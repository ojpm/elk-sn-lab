# Architecture overview for lab

## Purpose of integration

The Observability team has requested integration to the Servicenow CMDB, to "improve visibilty of system dependencies, ownership, and blast radius". The purpose of this lab is to get hands on with vendor documentation (referenced in the terms of reference readme), and validate this works as expected, and data sharing can be limited to only the data required to fulfil the Obs team request.

## Operational goals

- establish a functional integration from ELK to Servicenow, that is able to pull CMDB data for index environment
- validate and document the authentication mechanisms and requirements
- confirm that data extraction can be limited to required metadata only
- establish that the technical implementation can support the broader operational approach for work orchestration in Servicenow, and developer application investigation in ELK.

## Separation of concerns & Ownership boundaries

ServiceNow remains authoritative for:

- CMDB
- Incident management (including promotion of events to Incidents, and formal alerts/notification associated with Incident level events)
- Change management
- "orchestration of work/tasks"

ELK remains authoritative for:

- Developer daily work
- Developer Observability
- Search & Analytics
- 'Telemetry' correlation (accepting we primarily use logs only today)

Integration exists for operational enrichment only

## Not in scope

- No bidirectional synchronisation
- No workflow replication
- No ticketing replacement
- No full CMDB mirroring
