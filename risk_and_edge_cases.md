# Risk and Edge Cases

## Main Risks

### Duplicate Records

CRM syncs can create duplicate people or companies if matching rules are weak.

Risk control:

- store cross-system IDs
- match exact email first
- use normalized phone as secondary key
- route uncertain matches to manual review

### Overwriting Important Fields

Pipeline stage, owner, source, and notes may be manually managed.

Risk control:

- list protected fields
- require explicit overwrite rules
- avoid automatic updates on final statuses such as Won or Lost

### Appointment Timezone Errors

Appointment sync can break if systems use different timezones.

Risk control:

- confirm timezone source
- store ISO date/time where possible
- test appointment updates before go-live

### Sync Loops

Bidirectional sync can cause repeated updates between CRMs.

Risk control:

- use update source markers
- ignore automation-generated updates when needed
- add loop prevention flags

### Partial Failures

One system may update while the other fails.

Risk control:

- log each sync attempt
- include retry status
- avoid duplicate creation on retry
- keep manual recovery steps

### Unintended Outreach

CRM updates can trigger automations, campaigns, or follow-ups.

Risk control:

- disable outbound campaigns during testing
- use test tags
- require human approval before live sync
- document whether updates trigger messages

## Edge Cases to Ask About

- What happens if a contact changes email?
- What happens if two contacts share one phone number?
- What happens if one person belongs to multiple companies?
- What happens if a lead is deleted in one CRM?
- What happens if a contact unsubscribed or is marked do-not-contact?
- What happens if an appointment is rescheduled?
- What happens if an appointment is cancelled?
- What happens if API permissions are read-only?
- What happens if custom fields do not exist?
- What happens if a sales rep manually updates a synced field?

## Rollback and Safety Considerations

Before go-live:

- export a backup of affected test records
- keep a list of synced record IDs
- tag records created by the sync
- run a small test batch first
- keep outbound messaging disabled
- document how to undo created records

After go-live:

- monitor first runs manually
- review error logs
- check duplicate reports
- compare source and destination records
- pause sync if unexpected updates appear

## Recommended First Implementation Approach

1. Confirm one sync direction.
2. Create a field map.
3. Prepare 5-10 fictional test records.
4. Run in sandbox or test-only mode.
5. Review logs and duplicates.
6. Get client approval.
7. Enable limited live sync.
8. Expand only after stable results.
