# TrialPulse Requirements

## Personas

| Persona                     | Permission Set | Purpose / Responsibility                                                                                                                                                                                                        |
| --------------------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Clinical Operations Manager | Site Manager   | The primary business user concerned with monitoring enrollment performance and identifying sites falling behind. The need for earlier risk detection is stated in the blueprint; acting on alerts is a proposed interpretation. |
| Site Coordinator            | Coordinator    | Uses TrialPulse to view site progress and log new participants, as stated in Phase 1 of the blueprint.                                                                                                                          |
| Sponsor or QA viewer        | Read-only      | A read-only persona for viewing TrialPulse information. Specific viewing activities and responsibilities are not defined in Phase 2; oversight and quality assurance are proposed interpretations.                              |

**Source note:**

- **Directly specified in Phase 2:** The three persona names and their permission-set labels.
- **Also specified in Phase 1:** Clinical Operations Managers' enrollment visibility problem and coordinators' use of TrialPulse to view site progress and log participants.
- **Proposed interpretations:** Acting on risk alerts and the specific oversight activities of Sponsor or QA viewers.
- This section does not define object-level permissions, field-level permissions, or record-sharing rules. Those details are addressed separately in the security design.

## Functional Requirements

| ID    | Requirement                                                                                                               | Priority |
| ----- | ------------------------------------------------------------------------------------------------------------------------- | -------- |
| FR-01 | Trial records with a target enrollment, an enrollment window and a switch to enforce the cap                              | Must     |
| FR-02 | Site records with a lifecycle (Identified, Selected, Activated, Closed) and an enrollment target                          | Must     |
| FR-03 | Investigators linked to many sites through a junction object, with a role                                                 | Must     |
| FR-04 | Participant records with a system-assigned anonymous ID and a status lifecycle                                            | Must     |
| FR-05 | Participants can be created at, or moved to, only an Activated site                                                       | Must     |
| FR-06 | A trial-wide enrollment cap enforced on insert, update, transfer and undelete, with exact per-record errors               | Must     |
| FR-07 | Status transitions enforced from a configurable table                                                                     | Must     |
| FR-08 | A nightly job evaluates each Activated site's enrollment pace, with thresholds in custom metadata                         | Must     |
| FR-09 | A Platform Event is published when a site becomes At Risk                                                                 | Must     |
| FR-10 | A dashboard component lists sites with filters, progress, risk badge, open deviation count and a last-refreshed indicator | Must     |
| FR-11 | A participant entry component surfaces validation and cap errors clearly                                                  | Must     |
| FR-12 | Protocol deviations logged per site and counted on the dashboard                                                          | Should   |
| FR-13 | Errors from batch and partial-success DML written to an `Error_Log__c` object                                             | Must     |
| FR-14 | Three permission sets enforce the personas, and Apex runs with sharing and in user mode                                   | Must     |
| FR-15 | Data loads are idempotent through external IDs and upsert                                                                 | Should   |
| FR-16 | An inbound REST endpoint for status updates                                                                               | Nice     |
